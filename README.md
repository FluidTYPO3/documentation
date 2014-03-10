Fluid Powered TYPO3: Documentation
==================================

> Welcome to the brave new world of fully Fluid TYPO3 sites.

### Dear reader,

Thank you for your interest in the Fluid Powered TYPO3 project!

This documentation covers everything you could possibly wish to learn and know about templating with Fluid Powered TYPO3.

It is our hope that you find your new favorite templating workflow in this extension collection. We have tried to expect every
possible need you might have and deliver ways to achieve your goals in less time than ever before. Our extensions do the boring
work for you and enable you to focus fully on the templates themselves rather than how you should integrate them into TYPO3.

We do like to compare our extensions as a whole to the long time king of templating in TYPO3, TemplaVoila. However, there are some
diferences - and assuming you know how TemplaVoila works, the major differences from that to this family of extensions are:

* We don't store any form of templates in the database - they are instead contained in extensions called Provider Extensions.
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

#### Much more than pages and content

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
