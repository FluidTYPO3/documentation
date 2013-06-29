## Fluid Powered TYPO3: Best Practice for Migrations

> This chapter describes methods to migrate your existing templates to the current "best practice" state-of-the-art, how to change
> the database to accommodate things like renamed template files.

### Fluid Page Templates

> STUB: how to change oldschool TS setup to new location, recommendation for using an extension as provider, recommendation for
> using native view TS locations and the ext_tables.php code to register an extension.

> STUB: how to ensure page templates are in the right template location. Previously, you were a bit more free to use custom paths,
> now, you should always add page templates as if they belong to a "Page" controller and only add the templateRootPath up to where
> the "Page" folder is located.

> STUB: description of the connection between DB field values for selected page templates, the provider collection to which they
> belong and the location of the template file that is selected. This will allow more experienced users to figure out manual
> migrations or how to create those EXT update scripts for their own providers if they rename files fx.

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

> STUB: migrating ViewHelpers used in templates from FED to VHS counterparts - to be extended as we collect information, but should
> at least contain basic information about ViewHelpers like `fed:data.var` becoming `v:var.set` and `v:var.get` with each their
> own responsibility, and especially a section about ANY asset-type ViewHelper (fed's PageRenderer collection or script/style
> ViewHelpers for example) which should be converted to VHS assets.

> STUB: migrating other FED classes - this one may be a bit too exotic and/or related to antique code, but if you can, describe
> that most services and utilities from FED are now available as EXT:tool and most of them are drop-in replaceable by changing the
> class name that's injected or called statically. Not many changes have been made in EXT:tool since splitting it from FED.
