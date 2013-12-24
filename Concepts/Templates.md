Fluid Powered TYPO3: Concepts - Templates
=========================================

## Foreword

Templates rendered through Flux - which almost all templates in Fluid Powered TYPO3 are - are only slighly different from normal
Fluid templates. This concept description focuses on the differences and only mentions the rough outlines of many settings and
ViewHelpers which you can use to configure your templates. You should be at least somewhat familiar with how to use Fluid
templates in Extbase extensions before reading this.

This chapter deals only with best practice. There are multiple ways to use templates with Flux, each equally valid and functional,
but only one is considered best practice: [the Provider Extension](ProviderExtensions.md) which use the `FluidTYPO3\Flux\Core`
class to register which types of templates the Provider Extension contains.

### API level 1

This concept is one of the easiest to learn and use in everyday work. It requires almost no knowledge of TYPO3 and involves almost
no PHP code. It deals with template files and basic configuration files.

## What is a Flux template?

A template file which can be used by Flux is exactly the same as a normal fluid template with requirements:

1. The namespace `{namespace flux=FluidTYPO3\Flux\ViewHelpers}` must be present.
2. There must be a `Configuration` section created using `f:section`.

Inside the `Configuration` section (or whichever name you chose, in case you chose not to follow conventions) you should place
a `flux:form` tag containing at least an `id` attribute which should have a `lowerCamelCase` value, for example `specialImage`.

There is an optional section you can add when your Flux template is going to be used with the `tt_content` table - the section
called `Preview` (again, added using `f:section`) can contain HTML output that is displayed in the page module in TYPO3 when you
view that particular record. This works automatically for Flux-enabled plugins (which includes but is not limited to elements
for use in `fluidcontent`).

The final convention - which you *should* follow for transparency but which *can* be ignored when necessary, is to name the
section which contains the actual output rendering for the frontend, `Main` - this section gets rendered from the Layout you use,
which means you can of course choose a different name if that makes sense. Using the name `Main` simply means other people will
immediately know the purpose of that section.

> #### Customisation option
>
> The `Configuration` section name can be changed in your custom [Provider class](Providers.md). You can also use multiple sections
> and let your Provider switch between them based on, for example, a value from the record or from TypoScript, user session etc.

An extremely minimal template example (example: `fluidcontent` element template):

```xml
{namespace flux=FluidTYPO3\Flux\ViewHelpers}
<f:layout name="Content" />
<f:section name="Configuration">
    <flux:form id="myContentElement" />
</f:section>
<f:section name="Main">
    My content element
</f:section>
```

As you can see, this minimal example fulfills only the two main requirements above and uses name conventions for the Layout which
we describe in the next chapter. If added to a `fluidcontent` template collection, this template would result in one new content
element type named "myContentElement" which has no extra form fields, no special icon, no human-readable label and when rendered,
displays a simple "My content element" text without any HTML container element.

## Layouts and Partials

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

Partials are completely free to be used whenever you want, for whatever you want. They can contain shared Previews, common Flux
form fields, Grid definitions etc. - you simply have to place an `f:render` statement in the right place to "include" those.

## View path definitions in TypoScript

Flux will automatically look in the default template paths - even if you don't add any configuration - but you can (and indeed you
should) add your View configuration as TypoScript. You do this the same way you do it for any Extbase/Fluid extension so we won't
cover that here. You can find a bit more information about this particular TypoScript in the [ProviderExtension concept](ProviderExtensions.md).

Flux-enabled extensions will like any Extbase extension find template paths setup in `plugin.tx_yourextwithoutunderscores.view`.

Changing the view paths in TypoScript affects Flux just like it would affect any other extension. Flux will look in the new path
instead of the old one. Here it is worth noting that you can use so-called template path "overlays" - you can read more about how
these are set up, in [the documentation for EXT:view about path overlays](https://github.com/FluidTYPO3/view#template-path-overlays).
The same procedure is supported by `fluidcontent`, `fluidpages` etc. - and indeed, any extension in which you use Flux gets this
support without the need to install EXT:view. To get overlay capabilities for non-Flux extensions, simply install EXT:view and
follow the same procedure.

### Alternative to templates

> Careful! Some features from Fluid Powered TYPO3 will require template files containing a `Configuration` section and a
> `flux:form` tag. While it is possible to get around even this requirement, it involves creating a custom Provider to take the
> place of a Provider included with the extension, e.g. `fluidcontent` ContentProvider, `fluidpages` PageProvider, and so on.
> By replacing Providers you can change almost all aspects of how a certain feature works, fitting it to your site in exactly the
> right way. We designed the API so that features like `fluidcontent` all have the exact same level of API access that your own
> Providers do, which means anything these extensions can do yours can, too.

Note that there is an alternative to placing your form and fields inside the template. When you use a custom [Provider](Providers.md)
class you can have this class return PHP objects directly from the `getForm` and `getGrid` methods. Your Provider will have
inherited those two methods from Flux's `AbstractProvider` class - and in the code inherited from Flux, your Provider will by
default look into the template file for a Form and Grid instance. When you override either of these methods, reading the Form and
Grid respectively, does not have to require reading a template file.

What this means is that if you return native PHP objects from a Provider for both the Form *and* Grid instances, your template
file itself no longer is required to contain a `Configuration` section and any `flux:form` definition. Your Provider can even
return a raw HTML preview - which when done, lets you skip the `Preview` section as well. Of course, Previews only apply to
`tt_content` records as already mentioned.

To learn more about your options in this area you should read the [Provider class concept](Providers.md). Note that Providers
belong in the **layer 2** API and as such are harder to learn than using simple templates.
