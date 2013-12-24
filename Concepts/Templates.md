Fluid Powered TYPO3: Concepts - Templates
=========================================

## Foreword

Templates rendered through Flux - which almost all templates in Fluid Powered TYPO3 are - are only slighly different from normal
Fluid templates. This concept description focuses on the differences and only mentions the rough outlines of many settings and
ViewHelpers which you can use to configure your templates. You should be at least somewhat familiar with how to use Fluid
templates in Extbase extensions before reading this.

## What is a Flux template?

A template file which can be used by Flux is exactly the same as a normal fluid template with two requirements:

1. The namespace `{namespace flux=FluidTYPO3\Flux\ViewHelpers}` must be present.
2. There must be a `Configuration` section created using `f:section`.

The `Configuration` section name can be changed in your custom [Provider class](Providers.md). You can also use multiple sections
and let your Provider switch between them based on, for example, a value from the record or from TypoScript, user session etc.

There is an optional section you can add when your Flux template is going to be used with the `tt_content` table - the section
called `Preview` (again, added using `f:section`) can contain HTML output that is displayed in the page module in TYPO3 when you
view that particular record. This works automatically for Flux-enabled plugins (which includes but is not limited to elements
for use in `fluidcontent`).

The final convention - which you *should* follow for transparency but which *can* be ignored when necessary, is to name the
section which contains the actual output rendering for the frontend, `Main` - this section gets rendered from the Layout you use,
which means you can of course choose a different name if that makes sense. Using the name `Main` simply means other people will
immediately know the purpose of that section.

## The Layout

Just like in any other Fluid template you can use any Layout name you choose. We on the Fluid Powered TYPO3 team suggest you use
the names `Default`, `Content`, `CoreContent` and `Page` as needed. This list shows when to use which name:

* `Page` when the Layout is to be used with Page templates through `fluidpages`. It is suggested that all page templates share a
  common Layout file, but sometimes you need to add other Layouts, in which case names like `FrontPage` and `SubPage` and so on,
  will make a lot of sense to use.
* `Content` when the Layout is for `fluidcontent` elements - normally, you only need one Layout for content elements but like Page
  templates, you can split content element layouts into fx `MediaContent`, `TextContent` etc. Layout files.
* `CoreContent` when the Layout is for `fluidcontent_core` elements. You should not ever diverge from this convention for this
  particular extension - consistency and with it predicactability is **very** important for core content templates.
* `Default` when for example all your Layout file contains is an `<f:render />` statement rendering one section. If your Layout
  HTML is this simple, sharing a Layout file gives you a small performance boost and increases transparency - compiling one file
  is naturally faster than multiple files, and having one file to look at greatly increases your chance to find the right one ;)

Other than these naming conventions there are no particular rules or recommendations for Layouts set by Fluid Powered TYPO3. It is
completely up to you as developer/designer to decide what, if anything, your Layouts should contain.
