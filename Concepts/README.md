Fluid Powered TYPO3: Concepts
=============================

> This section of the documentation explains all concepts used in the extensions provided by the Fluid Powered TYPO3 team.

You will find details about the following concepts - the further down the list you go, the more advanced the concepts get. When
you start off using Fluid Powered TYPO3, you will likely only need to know a few of these concepts. But the more you use the
extensions, the more likely it is you will at some point want/need to use other concepts.

The information is in-depth; if you are just now starting, then you should start with the [Beginner's Guide](../BeginnersGuide.md)
and come back to this section if and when you need more detailed information about the concepts covered in that guide.

### Table of contents

* [Provider Extensions](ProviderExtensions.md)
  Is where you will find information about the preferred, always-recommended template storage: TYPO3 extensions which contain the
  templates your site should use.
* [Templates](Templates.md)
  Is where you find the low-down on how Fluid templates are used by Fluid Powered TYPO3.
* [Flux Forms](FluxForms.md)
  Is all about the forms you can make with Flux - different ways to create them, how they work together with TYPO3, understanding
  the structure of forms and how the different components fit together.
* [Flux Controllers](FluxControllers.md)
  Is a chapter dedicated to explaining how a Flux-enabled Extbase controller differs from a standard Extbase controller and how it
  opens up the Flux API for more customised uses.
* [Providers](Providers.md)
  Deals with the special class pattern called `Providers` - classes which provide data and manipulation for one very specific type
  of record - fx triggering on and then processing a `tt_content` record which has `CType` set to `myspecialtype` when saved; and
  much, much more - this concept is, together with the `Flux Forms` concept, the very core that allows Flux to do what it does.
* [Outlets and Pipes](OutletsAndPipes.md)
  Is dedicated to a special concept which can be used as standalone or in connection with a `Flux Controller` or `Provider`. In
  all its simplicity it two special types of classes which when put together, routes and processes data through a series of input
  and output `Pipes` to validate, convert, manipulate, store, send, log, publish (and more!) data from any source to any output.
