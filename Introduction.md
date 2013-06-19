## Fluid Powered TYPO3: Introduction

> Welcome to the brave new world of fully Fluid TYPO3 sites.

Dear reader,

Thank you for your interest in the Fluid Powered TYPO3 project!

It is our hope that you find your new favorite templating workflow in this extension collection. We have tried to expect every
possible need you might have and deliver ways to achieve your goals in less time than ever before. Our extensions do the boring
work for you and enable you to focus fully on the templates themselves rather than how you should integrate them into TYPO3.

We do like to compare our extensions as a whole to the long time king of templating in TYPO3, TemplaVoila. However, there are some
diferences - and assuming you know how TemplaVoila works, the major differences from that to this family of extensions are:

* We don't store any form of templates in the database.
* We don't use template mapping. When you change a template, it's changed - no need for remapping, storing header code and so on.
* We use TypoScript solely for configuration options, not for creating menus and such - we have ViewHelpers for that.
* We detect your collection of templates in a filesystem path and simply read all (enabled) files therein as one group.
* We store backend layout grids, nested content element grids, extra configuration fields and much, much more directly in the
  individual template files.
* We use Fluid for everything so that you can use Fluid in everything: for processing labels for your content elements, for
  generating an icon for a page template, and much more. This includes using your own ViewHelpers.
* We use a special form strategy which mimics FlexForms, we call this the Flux form. It creates a form inside content or page (or
  any other type of) record, into which you can insert fields simply by using one ViewHelper tag per field - which is extremely
  compact compared to traditional FlexForms.

Fluid itself is an extremely versatile templating language capable of outputting multiple formats and using highly advanced helper
tags - so-called ViewHelpers - to format and otherwise process output. It contains ways to loop through arrays, make conditions,
divide templates into smaller Partial templates, use special formatters and much more. Fluid even supports autocompletion when
you edit it in an editor which supports XSD Schemas and use a namespace in your templates...

...and the core concept of all the Fluid Powered TYPO3 extensions is quite simply that one template file becomes one page template
or content element type or even backend module. When you configure a path to a folder containing such template files, the
extension responsible for processing that type of template (Content, Page, Backend etc.) parses the file to read things like which
icon to use, which label to display, how (if any) a Grid should be displayed and any additional fields that should be displayed
to the editor.

### The new way

Fluid Powered TYPO3 encourages a new approach when creating the templating assets for your sites:

* We recommend using an extension to store all your files: templates, icons, images, stylesheets, scripts and all. This approach
  allows a few clever conventions to do most of the referencing. For example, your LLL files can reside in standard locations and
  your translation function calls can be much shorter. You can in almost all cases still reference your files using EXT:myext...
  paths if you need to.
* We recommend using ViewHelpers in the template when you want to render menus, output content, include styles/scripts and much
  more - almost everything you used to place in TypoScript you can now place in Fluid templates. Especially one helper extension
  in the family enables you to be much more creative with your templates - the extension called VHS (extension key `vhs`) which
  contains hundreds of general purpose ViewHelpers.
* We recommend using known Extbase conventions for placing configuration files, language files, private/public resources, class
  files and in short, everything that you are building.

The main difference lies in the recommended use of an extension to store your files. We call this particular type of extension
a "Provider Extension" - it "provides templates and configuration for those templates". A Provider Extension may still contain
the usual Extbase plugins, static TypoScript and such, but should prepare to respect a few reserved controller names and therefore
also template locations:

* The `Page` Controller name and template path is reserved for page template files using EXT:fluidpages.
* The `Content` Controller name and template path is reserved for content element template files using EXT:fluidcontent.
* The `Backend` Controller name and template path is reserved for backend module template files using EXT:fluidbackend.

This approach is much different from what you are used to from TemplaVoila but is easily understood (or learned by example; good
places to learn this way are the generic provider extensions you get from Fluid Powered TYPO3). Once you get used to this other
approach which focuses on files instead of database configurations; of editing in your editor instead of through the TYPO3 backend
and of using version control software on your template files and assets (don't despair - you can always learn this last bit once
you are comfortable with the new way of doing things).

### Why is this way better?

There are quite a few ways why we thing this new way is a lot better than the old way. Among our favorite reasons are:

* It's better to use Fluid to define additional, flexible fields for a record than to use a traditional FlexForm. Not only does
  it become dynamic (conditions, loops etc. become possible) - it is also around 10x more compact than FlexForm XML and the
  difference just increases as complexity rises: your Flux field is still one ViewHelper tag but the XML could be 30+ nodes.
* It's better to use files than database storage for a site's design. It's less error prone (remember those UTF8-upgrades that
  broke your TemplaVoila template mappings...?) and allows for much better version control and conflict resolution when multiple
  authors work on the same template file.
* It's better to use an extension to store your files. This makes it far more portable and version control becomes a breeze. Just
  to name one benefit of this: you can download the extension from your site, make any changes you need and re-upload it with
  overwriting enabled - which is a far more efficient way to deploy a new design than having to merge database differences between
  a development and production site.
* It's better to use a Fluid ViewHelper to render a menu object. Not only does this make it much easier to render different menus
  in different templates (including templates provided by other extensions!) without the need for a massive TypoScript object to
  be included with the template.
* It's better to use ViewHelpers rather than TypoScript objects to render dynamic or otherwise complex content and page template
  parts. This makes it much more transparent - you don't have to investigate the TypoScript to find out for example if a condition
  there has replaced some content in your template. Debugging is also easier: you simply use `<f:debug>` in your template and it
  becomes immediately visible which variables are available and how they would affect rendering.

But really, what we consider the main benefit is that we get to use Fluid for every single design and layout aspect of our sites.
And that we don't have to spend a lot of time worrying about integration - now, much more than before, we can focus on the core
task of creating excellent designs, useful and flexible content elements and much more.

It also doesn't hurt that there are many helper extensions which make it easier to work with Fluid. There are ViewHelper
collections as well as special feature extensions such as EXT:view which allows you to use overlay template paths (instead of
replacing an entire set of templates in a path, just the files which exist in the overlay are overridden - the rest are simply
taken from the original location).

We can't wait to tell you about all the improvements you can make to your templating workflow but would much rather show you
instead. So to get started quickly or just take a little peek at what Fluid Powered TYPO3 can do for you, check out the
[Live Bootstrap-based Introduction Package Demo](http://bootstrap.typo3cms.demo.typo3.org/) - follow the links to download the
package yourself, prepare a virtual host in your favorite web server software and unwrap. It is extremely easy to dive right into
the really interesting part: the templates. And your changes have immediate results.

Hope you enjoy!
