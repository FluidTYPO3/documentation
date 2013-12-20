Fluid Powered TYPO3: Concepts - Provider Extensions
===================================================

## Foreword

This concept description deals only with the current best practice. The legacy approach - placing template files in `fileadmin` as
was done in TemplaVoila for example - is considered deprecated and is not documented here. The new approach - placing templates in
an extension - is what this chapter is all about.

### Disambiguation

The concept of [Provider Extensions](ProviderExtensions.md) (this document) should not be confused with the concept of
[Providers](Providers.md). The Provider extensions concept is about file structure, the Provider concept is about a type of class.

### API level 1

This concept is one of the easiest to learn and use in everyday work. It requires almost no knowledge of TYPO3 and involves almost
no PHP code. It deals with template files and basic configuration files.

## What is a Provider Extension?

A Provider Extension is simply an extension which provides (as in: contains, configures and announces) templates and code which
Flux can then read. The information Flux reads can then be used by other features such as `fluidpages` and `fluidcontent`. The
idea behind a Provider Extension is to have a structure for templates that is based on well-known conventions.

## Why an extension - why not just store files somewhere?

While we realise that this is an opinionated way, and in addition a way which was not used in the old TemplaVoila days, we hope
the following points will convince you exactly why this is a much better approach than files in fx `fileadmin`:

1. An extension is very easy to reuse. Download it from one site and upload to another - and when the template contains both
   templates _and_ the configuration required to use them, your template collections can be used in a lot more places than just
   the site they were made for (case in point: `fluidcontent_bootstrap` is a Provider Extension specifically designed to be used
   on site after site, when the site is based on Twitter's Bootstrap CSS framework - it contains content specific to Bootstrap).
2. An extension in the eyes of TYPO3 is a **logical context** - extensions are identified by a unique key. This clearly separates
   one template collection from other collections. This is different from a convention of fx having templates in `fileadmin/html`
   but it does mean that you can run multiple collections alongside each other without risk of colissions. And, as mentioned
   already, this also means your templates become portable.
3. Packaging into extensions gives Flux a way to build a proper `ControllerContext` to use when accessing your templates - and
   this, among other things, means you can use short LLL label names and Fluid will automatically use the correct LLL file. The
   same applies to resource locations - for example images, styles and scripts used in your templates. Having a proper context
   means Fluid and Extbase both "just know" where to look for files. Knowing the extension key means knowing which paths to use.

Although there are of course many more points (version control efficiency, TS configuration conventions, view paths overrides etc)
these three should be enough to convince you that starting to use extensions instead of files in `fileadmin` will, very quickly
even, save you a lot of time especially when using resources (including LLL).

But if that's not enough: other features Fluid Powered TYPO3 provides assume you already use an extension - and we have designed
it all to fit together so that your Provider Extension may contain content and page templates, custom plugins, backend modules
etc. as one combined package - obviously ideal as a way to ship entire site designs along with all configuration and with all
dependencies set as extension dependencies that the TYPO3 extension manager will install.

This explains why our best practice - and indeed the only way we bother documenting in detail - is the Provider Extension. It is
the only approach that fits perfectly and it provides so many benefits that it would require severe TYPO3 hacks to get otherwise.

## The file layout

Files in a Provider Extension are by convention always in this format:

```
typo3conf/ext/myproviderextension
__ Configuration
____ TypoScript
______ constants.txt
______ setup.txt
__ Resources
____ Private
______ Language
________ locallang.xlf
______ Layouts
______ Partials
______ Templates
________ Content
________ Page
____ Public
______ Javascript
______ Stylesheet
______ Images
__ ext_emconf.php
__ ext_icon.gif
__ ext_tables.php
__ ext_localconf.php
```

It should be pretty clear just from this that we _use Extbase's conventions to place configuration and template folders and files_
and that _your provider extension is simply a standard TYPO3 extension with some predefined file structure_. The way you use each
folder normally depends on which type of Provider Extension you are creating - and the feature-specific guides always start by
explaining which folders and files are involved.

Your `ext_localconf.php` and `ext_tables.php` files will contain the various calls to Flux to register which types of Flux
templates your Provider Extension uses - along with any other configuration you might need; custom TCA fields for `tt_content` fx.
The `Resources/Private` folder contains a template structure and the language file - and the templates are divided into subfolders
by name of the controller which will render them (e.g. in `fluidpages` the controller is named `PageController` which means the
folder name is `Page`, in `fluidcontent` the controller name is `Content` and so on). Layouts and Partials are supported just like
normal. And finally, the `Configuration` directory contains the TypoScript that your extension makes available for integrators to
select and use in TYPO3 when your Provider Extension is installed.

## What must the template files contain?

The actual contents of template files depends on which feature they should be used with - but there are a few shared rules which
you can read all about in the [Templates concept](Templates.md) chapter of this documentation. Following the standard rules always
results in a working, selectable (yet very minimal) template - what the template should contain in addition to the standard format
is up to each specific feature and is documented by each feature.

The only files which are required to contain the standard format, are the files located under `Templates` - in other words: your
Layouts and Partials can contain any content you want, but Templates must contain Flux-specific information.

Study the [Templates concept](Templates.md) for more details about the content of these template files.

## Installing and using Provider Extensions

Just like a normal TYPO3 extension, you install Provider Extensions through the extension manager - and you then include the
static TypoScript, if static TypoScript is provided by the extension. Some Provider Extensions may require you to configure the
extension (click the gear icon in extension manager next to that extension) and others may require you to configure TypoScript
constants or setup. However, configuration is by convention always provided in one or both of these ways.

Using the Provider Extension depends on the feature it provides templates for - `fluidpages` for example lets you select the
templates from Provider Extensions when you edit page properties, `fluidcontent` lets you add content elements which render each
of the `Content` templates you provide. Other features let you select, configure and render templates in different ways - each
feature documents how exactly they will use the templates from your Provider Extension and what the templates should contain.

## Custom controllers in Provider Extensions

Some features of Fluid Powered TYPO3 - such as `fluidcontent` and `fluidpages` - support custom controller classes which Flux will
use instead of the controllers that ship with each of those features. Naturally this allows much more flexibility (as an example,
a custom `PageController` can accept custom arguments in the action that corresponds to the template that gets rendered). If and
when you add custom controller classes the location and naming of these also must follow Extbase conventions:

```
typo3conf/ext/myproviderextension
__ Classes
____ Controller
______ PageController.php
______ ContentController.php

```

The [Flux Controller concept](FluxControllers.md) applies here and the linked document contains in-depth information about these
custom controller classes and what they can do. However, that concept belongs in **layer 2** which is obviously more complex to
learn and use, than the basic templates-and-ViewHelpers-and-TypoScript approach.

## Adding custom plugins in Provider Extensions

We explicitly state this since it is not immediately obvious: you can of course also place Extbase plugins alongside the templates
and configuration your Provider Extension contains - and the plugins and templates can even use the same TypoScript setup since
they share the same extension scope, as already explained.

The only thing you must be careful of, is not to use the reserved `Content`, `Page`, `CoreContent` or `Backend` controller names
if (and only if) you intend to use either of those features - from `fluidcontent`, `fluidpages`, `fluidcontent_core`, and
`fluidbackend` respectively. If you don't use a particular feature you can of course grab the reserved controller name for your
controller - but it is recommended to avoid these names if you are not sure if you may be using other Fluid Powered TYPO3 features
later on in your Provider Extension.
