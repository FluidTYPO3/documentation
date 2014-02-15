## Fluid Powered TYPO3: Best Practice for Configuration

Before you read this chapter there are a few basic facts you must know. The extensions in this family (at least the feature
providers: fluidcontent, fluidpages, fluidbackend) all use TypoScript Fluid view configurations for template paths.

Templates get detected on a file basis (by filtering through files of a certain format - html, xml etc) and then parsed.
This reads the template's configuration options (such as the translated label or icon it uses).

There are many ways to set your template paths but only one which builds on Extbase/Fluid conventions. All methods are in
this chapter, along with their implications, so you can match them to your use case and use the right one.

When you use these conventions the resulting code structures become easy to understand. Even if you don't publish your
work, you should still use the conventions if nothing else for your own benefit when later revisiting code.

### Extension Key Registration (encouraged)

> This is the preferred way to integrate with fluidcontent/fluidpages/fluidbackend and so on. It ensures a proper relation to an
> extension and uses all known conventions from Extbase. For example, about where to expect template file locations, public assets etc.
> The conventions for this particular type of integration we've placed in a dedicated chapter in this file.

This approach assumes you have an extension. It can be one which already contains site-oriented files for your specific site or
you can just create one (See the chapter on "Building Code"). In your ext_tables.php file add the following registration code:

```php
// $_EXTKEY = 'myext';
Tx_Flux_Core::registerProviderExtensionKey($_EXTKEY, 'Content'); // to register content templates
Tx_Flux_Core::registerProviderExtensionKey($_EXTKEY, 'Page'); // to register page templates
Tx_Flux_Core::registerProviderExtensionKey($_EXTKEY, 'Backend'); // to register backend module templates
```

#### TypoScript configuration convention

To add TypoScript configuration (such as the mandatory View paths) use the Extbase convention:

```php
// in ext_tables.php
t3lib_extMgm::addStaticFile($_EXTKEY, 'Configuration/TypoScript', 'Description of configuration');
```

Which then expects a `setup.txt` and optional but also recommended `constants.txt` to be in the
`EXT:myext/Configuration/TypoScript` folder. We encourage you to use constants when working with configuration-type
TypoScript like max widths, heights, colors etc.

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

#### Implications

The second parameter indicates the Controller name. This has the following implications:

* The expected Controller class name, if one exists, is Tx_Myext_Controller_ContentController. If this class exists we use it.
  when rendering your template file - note that this is the exact opposite of the usual Extbase plugin logic in which you would
  call your controller which then tries to find a corresponding template. In this case we find the template's connection to a
  Controller action which we call instead of using the built-in rendering exists.
* A TypoScript setting should exist in `plugin.tx_myext.view.templateRootPath`. If your templates use these, add
  paths for Partials and Layouts as well.
* The template folder is `Resources/Private/Templates` in your extension. Or another path, if you override it in TypoScript.
  Note: to place your files in `fileadmin´ (which we discourage) see the chapter custom paths.
* If your templates use TypoScript, it should be in `plugin.tx_myext.settings`. It will then be available in Fluid as
  the "magic" template variable {settings} which you do not need to pass to Partials etc. to use it.

In all ways this is the preferred method of integration. It requires you to store your templates and asset files in an extension,
which is a bit of a break if you were before used to storing them in fileadmin (as with TemplaVoila) but consider for a moment
the consistency you gain. The configuration paths follow every convention known from Extbase and Fluid, which means you also get
the ability to add language files without using long paths to each file. There's of course more benefits from doing it this
way - more than can fits here - _please consider using this approach even if you are just making basic templates._

### Simple Template Path Registration (discouraged)

> We discourage this method since it detaches your template files from an extension scope. You should store your files
> and configuration in an extension - it makes it easier to transport and allows you to use even more conventions. And it makes the
> code you need to write, smaller, and the usability (for integrators) of your work a lot higher.

To use a dumb path for your templates, for example in `fileadmin`, add the following (to your existing TypoScript
configuration records):

```txt
# for fluidpages template paths
plugin.tx_fluidpages.collections.mycollectionname.templateRootPath = fileadmin/templates/page/
# for fluidcontent template paths
plugin.tx_fluidcontent.collections.mycollectionname.templateRootPath = fileadmin/templates/content/
```

> Note: EXT:fluidbackend __does not support TypoScript paths this way. It requires the use of an extension - see above__.

#### Implications

When you register template paths without an extension to contain them, you lose the following:

* You cannot use `<f:translate>` and other ViewHelpers depend on an extension context. When using such ViewHelpers you must
  for example use a full LLL:... path in the `key` attribute, in the case of `<f:translate>`.
* You cannot use TypoScript settings without `<v:var.typoscript>` to read them, which adds overhead.
* You cannot override LLL labels through TypoScript which you would be able to do if the LLL was in an extension.
* Templates render in context of the feature extension - e.g. fluidcontent, fluidpages, fluidbackend.

### Legacy

The fluidpages and fluidcontent extensions still, for legacy reasons, support these configuration paths:

```txt
plugin.tx_fed.fce.mycollectionname.templateRootPath = ...
plugin.tx_fed.page.mycollectionname.templateRootPath = ...
```

No schedule for removal exists at the time of writing this, but removal will happen.

Please upgrade them to either use the "Simple Template Path Registration" paths (e.g.
`plugin.tx_fluidpages.collections.mycollectionname.templateRootPath` etc) which is compatible. Or if possible, transfer them to an extension and
use the recommended extension key based registration.
