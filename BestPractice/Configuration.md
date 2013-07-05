## Fluid Powered TYPO3: Best Practice for Configuration

Before you read this chapter there are a few basic facts you must know. The extensions in this family (at least the feature
providers: fluidcontent, fluidpages, fluidbackend) all use TypoScript Fluid view configurations to read a path to the templates
which should be scanned.

Templates are then scanned on a file basis (by simply filtering through files of a certain format - html, xml etc) and parsed in
order to read the actual per-template configuration options (such as the human readable label or icon for this template).

There are multiple ways to set your template paths but only one preferred which builds solely on Extbase/Fluid conventions. All
methods are described in this chapter, along with their implications so you can match them to your use case and use the right one.

### TypoScript configuration convention

> Note: This convention applies only when you use an extension to store your templates and configuration, which is highly
> recommended - and the officially preferred way.

In order to use basic TypoScript configuration (such as the mandatory View paths) you should use the TYPO3/Extbase convention:

```php
// in ext_tables.php
t3lib_extMgm::addStaticFile($_EXTKEY, 'Configuration/TypoScript', 'Description of configuration');
```

Which then expects a `setup.txt` and optional but also recommended `constants.txt` to be placed in the
`EXT:myext/Configuration/TypoScript` folder. You are encouraged to use constants - you are always encouraged to do this when
working with configuration-type TypoScript (as opposed to for example when you create COA objects). Since almost all TypoScript
involved with any extension in this family is exactly configuration-type TypoScript, you should simply make a habit of using it
always, for all of page-, content- and backend module templates.

Your settings should use the conventional scopes:

```txt
# for frontend-related extensions
plugin.tx_myext.view {
	templateRootPath = ...
	...
}
plugin.tx_myext.settings {
	foo = bar
}

# for backend (EXT:fluidbackend currently) oriented extensions
module.tx_myext.view {
	...
}
module.tx_myext.settings {
	foo = bar
}
```

When you use these conventions the resulting code structures become universally understandable - even if you don't publish your
work, you should still use the conventions if nothing else for your own benefit when later revisiting code.

### Extension Key Registration

> This is the preferred way to integrate with fluidcontent/fluidpages/fluidbackend and so on - it ensures a proper relation to an
> extension and uses all known conventions from Extbase regarding where to expect template file locations, public assets etc.
> The conventions for this particular type of integration are listed in a dedicated chapter in this file.

This approach assumes you have an extension. It can be one which already contains site-oriented files for your specific site or
you can just create one (See the chapter on "Building Code"). In your ext_tables.php file add the following registration code:

```php
// $_EXTKEY = 'myext';
Tx_Flux_Core::registerProviderExtensionKey($_EXTKEY, 'Content'); // to register content templates
Tx_Flux_Core::registerProviderExtensionKey($_EXTKEY, 'Page'); // to register page templates
Tx_Flux_Core::registerProviderExtensionKey($_EXTKEY, 'Backend'); // to register backend module templates
```

#### Implications

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

### Simple Template Path Registration

> This method is discouraged as it detaches your template files from an extension scope. You are encouraged to store your files
> and configuration in an extension - this makes it easier to transport and allows you to use even more conventions to make the
> actual code you need, smaller, and the usability (for integrators) of your work a lot higher.

To use a dumb path for your templates for example in `fileadmin` simply add the following TypoScript (to your existing TypoScript
configuration records):

```txt
# for fluidpages template paths
plugin.tx_fluidpages.collections.mycollectionname.templateRootPath = fileadmin/templates/page/
# for fluidcontent template paths
plugin.tx_fluidcontent.collections.mycollectionname.templateRootPath = fileadmin/templates/content/
```

> Note: EXT:fluidbackend __does not support TypoScript paths this way; it requires the use of an extension - see above__.

#### Implications

When you register template paths this way, without an extension being associated with them, you lose the following possibilities:

* You cannot use `<f:translate>` and other ViewHelpers which use an extension context to determine which files to use; when using
  such ViewHelpers you are required to use a full LLL:... path in the `key` attribute.
* You cannot use TypoScript settings unless your templates make use of `<v:var.typoscript>` to read them, which adds overhead.
* You cannot override LLL labels through TypoScript which you would be able to do if the LLL was in an extension.
* Templates will render in the scope of the extension which renders the template - e.g. fluidcontent, fluidpages, fluidbackend.

This may or may not present problems for your strategy - but even if you consider it acceptable to lose these capabilities, please
do consider using an extension instead. It is, in every way, better in the long run and presents less hoops to jump through.

### Legacy

The fluidpages and fluidcontent extensions still, for legacy reasons, support these configuration paths:

```txt
plugin.tx_fed.fce.mycollectionname.templateRootPath = ...
plugin.tx_fed.page.mycollectionname.templateRootPath = ...
```

These are considered deprecated. No schedule for removal exists at the time of writing this.

Whenever you see these, please change them to either use the "Simple Template Path Registration" paths (e.g.
`plugin.tx_fluidpages.collections.mycollectionname.templateRootPath` etc) or if at all possible, transfer them to an extension and
use the recommended extension key based registration.
