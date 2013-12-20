Fluid Powered TYPO3: Concepts
=============================

## Introduction

This section of the documentation explains all concepts used in the extensions provided by the Fluid Powered TYPO3 team.

You will find details about the following concepts - the further down the list you go, the more advanced the concepts get. When
you start off using Fluid Powered TYPO3, you will likely only need to know a few of these concepts. But the more you use the
extensions, the more likely it is you will at some point want/need to use other concepts.

The information is in-depth; if you are just now starting, then you should start with the [Beginner's Guide](../BeginnersGuide.md)
and come back to this section if and when you need more detailed information about the concepts covered in that guide.

## Table of contents

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
* [Outlets and Pipes](OutletsAndPipes.md) **Since Flux 7.0.0**.
  Is dedicated to a special concept which can be used as standalone or in connection with a `Flux Controller` or `Provider`. In
  all its simplicity it two special types of classes which when put together, routes and processes data through a series of input
  and output `Pipes` to validate, convert, manipulate, store, send, log, publish (and more!) data from any source to any output.

## API levels

The handful or so of concepts that Flux introduces can be divided into _levels_, each _level_ increasing the complexity and also
the flexibility. The documents in this section of the documentation mention the _API level_ of a feature - here is what those
levels mean:

1. The outer layer, **layer 1**: ViewHelpers, TypoScript, template files. Low complexity.
   Basically this layer contains _form field setups, configuration and rendering_ and is the simplest layer. It is used by site
   integrators to influence where to look for template files, for example. And by template designers and developers to define a
   collection of form fields that are available when editing custom content elements, pages, etc.
2. The inner layer, **layer 2**: Classes, Controllers, Services - "internals". Medium complexity.
   The more advanced ways you can integrate with Flux are opened up in PHP - they are (generally speaking) not available to be
   used through TypoScript or in Fluid templates. Since this layer uses PHP only, it requires a fair bit of knowledge about making
   Extbase classes (or a brain tuned perfectly to learning by example, because there are plenty examples in Fluid Powered TYPO3's
   extensions in the form of features like `fluidpages`).
3. The core layer, **bottom layer**: For experienced developers. High to very high complexity.
   This layer is normally one you never have to touch. There are almost no features as such at this layer, but there are ways to
   influence Flux's logic in very advanced ways.

You can build very advanced sites using only **layer 1** features like `fluidcontent`, `vhs` and `fluidpages`. You can make some
advanced extensions using **layer 2** features like [Providers](Providers.md) and [Outlets/Pipes](OutletsAndPipes.md). And finally
you can manipulate almost everything integrated in **layer 1** and **layer 2** by practicing the dark arts of the **bottom layer**.

In daily work though, we don't speak of _API levels_ as such - we simply mention the integration context (template, controller,
TypoScript, etc). However, it is used in this section of the documentation to give you a very fast way to see _how difficult it
would be to learn and to use a certain feature at your skill level_.
