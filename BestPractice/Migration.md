## Fluid Powered TYPO3: Best Practice for Migrations

In case you started with EXT:fed some time ago, you will need to make some adjustments to port your existing FCE's and
page layouts to EXT:fluidcontent and EXT:fluidpages.

This chapter should give you an advice on how such migrations would work in general.

### First things first

FED/Flux in their old representation gave you the possibility to place your templates somewhere in the filesystem
and reference it via TypoScript. This concept CAN still be used, but the recommended pattern is to use a
"provider extension" to encapsulate your work on a specific topic.

The rest of this document assumes, you used a provider extension. Please consult
[the chapter on code generation](CodeBuilding.md) for further instructions on what options you have for autogeneration.

Your provider extension should at least consist of the following files:
```plain
Configuration
 -> TypoScript
    -> setup.txt
    -> constants.txt
ext_emconf.php
ext_tables.php
ext_localconf.php
```

For the sake of simplicity, we call the extension ``fluidtypo3_migration``.

#### TypoScript before the migration

In your old setup, you use something like this to register your elements to FED/Flux:
```
plugin.tx_fed.fce.mycollectionname.templateRootPath = ...
plugin.tx_fed.page.mycollectionname.templateRootPath = ...
```

#### TypoScript after migration

We like to follow standards. In history, ``flux`` had to find its way, but now, we use standard view registration in
every FluidTYPO3 extension. As you may have thought of, change your TypoScript from above to something like this:

```plain
# View Registrierung
plugin.tx_fluidtypo3migration.view {
  label = FluidTYPO3 Migration Elements
  extensionKey = fluidtypo3_migration # notice this entry. Comes in handy when using extension names with underscores
  templateRootPath = EXT:fluidtypo3_migration/Resources/Private/Templates/
  partialRootPath = EXT:flux/Resources/Private/Partials/
  layoutRootPath = EXT:fluidtypo3_migration/Resources/Private/Layouts/
}
```

#### Registering the provider extension

If you did not generate a provider extension but are in between a migration and still have an own extension you use, you
have to register it to flux:
According to the [chapter about configuration](Configuration.md#extension-key-registration) you will have to add code to
your ``ext_localconf.php`` *and* ``ext_tables.php``.

```php
// If your extension contains fluidpages page templates/layouts
Tx_Flux_Core::registerProviderExtensionKey('fluidtypo3_migration', 'Page');
// If your extension contains fluidcontent FCE templates:
Tx_Flux_Core::registerProviderExtensionKey('fluidtypo3_migration', 'Content');
// If your extension containts Backend modules:
Tx_Flux_Core::registerProviderExtensionKey('fluidtypo3_migration', 'Backend');
```

### Fluid Page Templates

> STUB: how to change oldschool TS setup to new location, recommendation for using an extension as provider,
> recommendation for using native view TS locations and the ext_tables.php code to register an extension.

> STUB: how to ensure page templates are in the right template location. Previously, you were a bit more free to use
> custom paths, now, you should always add page templates as if they belong to a "Page" controller and only add the
> templateRootPath up to where the "Page" folder is located.

> STUB: description of the connection between DB field values for selected page templates, the provider collection to
> which they belong and the location of the template file that is selected. This will allow more experienced users to
> figure out manual migrations or how to create those EXT update scripts for their own providers if they rename files
> fx.

### Fluid Content Templates

> STUB: old location to new pragma - same as for pages.

> STUB: page template locations - same as for pages, except enforcing the "Content" controller - note: this is quite different
> from earlier practices where an "Elements" or even "FCE" controller had been used in examples; it should be described how to
> rename these template folders and adjust the TS (which before, might have contained the "/Elements/" or "/FCE/" segments).

> STUB: connection between DB fields and provider collection and files.

### Flux-enabled plugins in general

> STUB: explanation about EXT:builder's CLI commands which can validate a template, ensuring it is fully checked for bad VH class
> names, missing/changed/now-required arguments and such.

> STUB: explanation about using EXT:builder to quickly generate a new skeleton extension which can receive template files from
> an older version extension which can sometimes be easier than renaming everything.

### From FED to ?

> STUB: migrating ViewHelpers used in templates from FED to VHS counterparts - to be extended as we collect information, but
> should at least contain basic information about ViewHelpers like `fed:data.var` becoming `v:var.set` and `v:var.get` with
> each their own responsibility, and especially a section about ANY asset-type ViewHelper (fed's PageRenderer collection
> or script/style ViewHelpers for example) which should be converted to VHS assets.

> STUB: migrating other FED classes - this one may be a bit too exotic and/or related to antique code, but if you can, describe
> that most services and utilities from FED are now available as EXT:tool and most of them are drop-in replaceable by changing
> the class name that's injected or called statically. Not many changes have been made in EXT:tool since splitting it from FED.
