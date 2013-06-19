## Fluid Powered TYPO3: Best Practice

Before you read this chapter there are a few basic facts you must know. The extensions in this family (at least the feature
providers: fluidcontent, fluidpages, fluidbackend) all use TypoScript Fluid view configurations to read a path to the templates
which should be scanned.

Templates are then scanned on a file basis (by simply filtering through files of a certain format - html, xml etc) and parsed in
order to read the actual per-template configuration options (such as the human readable label or icon for this template).

There are multiple ways to set your template paths but only one preferred which builds solely on Extbase/Fluid conventions. All
methods are described in this chapter, along with their implications so you can match them to your use case and use the right one.

### Extension Key Registration

> This is the preferred way to integrate with fluidcontent/fluidpages/fluidbackend and so on - it ensures a proper relation to an
> extension and uses all known conventions from Extbase regarding where to expect template file locations, public assets etc.
> The conventions for this particular type of integration are listed in a dedicated chapter in this file.

This approach assumes you have an extension. It can be one which already contains site-oriented files for your specific site or
you can just create one (See the chapter on "Building Code"). In your ext_tables.php file add the following registration code:

```php
// $_EXTKEY = 'myext';
Tx_Flux_Core::registerConfigurationProviderExtensionKey($_EXTKEY, 'Content'); // to register content templates
Tx_Flux_Core::registerConfigurationProviderExtensionKey($_EXTKEY, 'Page'); // to register page templates
Tx_Flux_Core::registerConfigurationProviderExtensionKey($_EXTKEY, 'Backend'); // to register backend module templates
```

The second parameter indicates the Controller name. This has the following implications:

* The expected Controller class name, if one exists, is Tx_Myext_Controller_ContentController. If this class exists it is used
  when rendering your template file - note that this is the exact opposite of the usual Extbase plugin logic in which you would
  call your controller which then tries to find a corresponding template. In this case, the template's connection to a Controller
  action is detected and if the action corresponding to the template exists, it is called instead of using the built-in rendering.
* TypoScript is expected to exist in `plugin.tx_myext.view.templateRootPath` at the very least. If your templates use them, add
  paths for Partials and Layouts as well.
* The expected template folder locations are in `Resources/Private/Templates` of your extension, or another if so changed in the
  TypoScript configuration for your extension. The convention suggests using the path as in the example and you are highly
  encouraged to use this location to make your extension much easier to understand. But you are free to use another location.
  Note: to place your templates in `fileadmin´ (which is discouraged but still possible) see the chapter about non-extension paths.
* If your templates use TypoScript, it should be placed in `plugin.tx_myext.settings` and will then be available in Fluid as
  the "magic" template variable {settings} which does not need to be passed to Partials etc. to be accessed.

In all ways this is the preferred method of integration. It requires you to store your templates and asset files in an extension,
which is a bit of a break if you were previously used to storing them in fileadmin (as with TemplaVoila) but consider for a moment
the additional consistency you gain. The configuration paths follow every convention known from Extbase and Fluid, which means
you also get the ability to add language files without using long paths to each file. There's of course more benefits from doing
it this way - more than can be mentioned here - _please consider using this approach even if you are just making basic templates._
