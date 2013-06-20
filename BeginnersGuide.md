## Fluid Powered TYPO3: Beginner's Guide

> This document describes the concepts and features found in the Fluid Powered TYPO3 extension family. You can read this guide
> as if it were a book about the thinking behind the features.

### A foreword about the Bootstrap Package

A special introduction package has been created by Fabien Udriot of [Ecodev](http://ecodev.ch) - this package showcases almost all
of the possibilities and many of the best practices for creating fluid pages and content elements.

Download the [Bootstrap Introduction Package](http://get.typo3.org/bootstrap) and unpack it, create an Apache virtual host for
the new site and point your web browser to the new site. Follow the install wizard and when you're done, come back to
this document. You can read the rest of this document while checking out the relevant parts of the newly installed site - doing it
this way makes a lot of sense since all the special settings, integrations and files which are mentioned in this guide are also
used in that package.

Besides this, the introduction package is a great teaching and study tool if you wish to study a current, best-practice setup.

# The Beginner's Guide

At first glance the Fluid integration features provided by the extensions in this family can seem overwhelming. But don't worry -
due to focus on separation of concerns it is easy to quickly understand the purpose of each extension.

The first three extensions you need to know are *Flux*, *Fluid Pages* and *Fluid Content*. Their extension keys are *flux*,
*fluidpages* and *fluidcontent*. You can read a little bit about each extension on this page - and there is much more
documentation about each, in other sections of this documentation repository.

In this document, all the concepts used in Fluid Powered TYPO3 extensions are explained one by one. Each concept is covered in a
section by itself, starting from the core concept and building outwards to each individual feature.

## Flux: Fluid FlexForms

> Fluid template based FlexForms using ViewHelpers for compact and dynamic configurations.

Flux lies at the core of every feature in the Fluid Powered TYPO3 family of extensions. Flux has a single purpose in life: to
allow a new type of very compact, very dynamic Flex Form to be integrated in a Fluid template. The result? You can add advanced
form sections with configuration options which content editors can fill out. By using Flux both Fluid Pages and Fluid Content
allow pages and content elements to be configured individually - while at the same time allowing the form fields to be defined
directly in the Fluid template for pages and content elements respectively.

If this sounds a bit abstract to you, imagine that you have a content element which requires a toggle - for example to turn on
and off a special output section in the content element's template. With traditional plugins you would either have to extend the
`tt_content` table or use a FlexForm (with a huge XML data structure just to add this one field, and registration of the
`pi_flexform` field). With Flux you can simply add a ViewHelper tag in your template's configuration and the resulting field is
displayed when a content element of that type is edited or created. This is very much like the way a traditional FlexForm works,
with the major difference being that you don't need to enable the field or create a special XML file for your FlexForm's data
structure - which is not only much more compact but also becomes dynamic since you are able to use Fluid's conditions, loops and
much more to affect your form's fields and structure.

Flux FlexForms are by design capable of containing a special Preview section (using f:section). This special section can contain
an HTML or plaintext representation of the content element, plugin settings, typoscript settings preview - or anything you desire.

Flux can also be used by any type of plugin (also pibase) to not only use these dynamic Fluid FlexForms - but also contain child
content elements (just like Fluid Content elements can). This child content feature is a sub function of Flux, one that consists
of a simple parent-child relationship column and a special Fluid Widget which allows the Preview section to contain a special
content grid into which content elements can be inserted.

### The juicy tech details about Flux

In order to understand how Flux forms work you must first understand the concept of FlexForms. These special "forms" are in fact
special field types supported natively by the TYPO3 core. The field uses a special XML structure file containing a list of fields
(and more special configuration such as sheets or sections with reusable objects - there's more on this later). What Flux does is
remove the need for an XML file - instead, Flux makes it possible to define the same structure as a special chunk of template
code exclusively using Fluid ViewHelpers. When used, the special ViewHelper types in Flux store the configuration (array form)
needed for TYPO3 to render a special "inline form" - a "FlexForm field".

Flux hooks into TYPO3 and is triggered whenever a field of the type "flex" (for example: `tt_content.pi_flexform` is such a field)
and instead of processing an XML file, Flux processes a Fluid template file and turns it into a data structure.

The integration with content and pages builds on *parsing the record from `tt_content` or `pages` and analysing the values of the
special selector fields* which are added to the tables' TCA configuration by fluidcontent and fluidpages respectively. This value
(in the context of pages, it can be inherited from parent pages) determines which template file contains the Flux form definition
as well as the actual rendering of the content element, page template, plugin view etc.

In order to do this (and a few other operations) Flux uses a special concept: the ConfigurationProvider pattern.

## What is a ConfigurationProvider?

A ConfigurationProvider is a special type of class which serves a specific purpose - much like an Extbase Controller class which
renders Reponses to Requests, a ConfigurationProvider returns values needed to identify a template file and configure the
rendering process. A ConfigurationProvider serves as the link between a record of a particular type and a Fluid template. Which
simply means that in order for Flux to know which template file it should render, it asks for a ConfigurationProvider to return
the template path and filename, variables which must be assigned to the template when rendering, paths to Layouts and Partials
the template should use and much more.

The ConfigurationProvider also serves as a *record processor* which means it is capable of manipulating records before they are
saved, whenever TYPO3 performs operations on these records. This includes when a record's values are updated, when a record is
deleted, moved, hidden and so forth.

In other - and much shorter words - the ConfigurationProvider has two purposes: one is to return a proper template and template
related configuration for Flux to use; the other is to configure records (if this is necessary). And every method which reads
these variables and processes records accepts the current record (as array) which allows the ConfigurationProvider to, just for
example, return a dynamic value for $templatePathAndFilename based on the current record being edited/rendered - which is exactly
how fluidcontent and fluidpages both work: by using a special field in the record, in which a value is stored that can be used
to resolve a template filename, a set of template paths and more.

### The practical example: Flux's standard ConfigurationProvider

The most basic ConfigurationProvider in Flux is attached to the `tt_content` table and triggers whenever content records are saved,
moved etc. The purpose of this ConfigurationProvider is to *adjust the position of content elements when they are saved in nested
content element containers also enabled by Flux* - it reacts to new records being created or records being drag-n-dropped into
content areas, then adjusts values in the record which define where the content element is located relative to the parent and area
name of that parent.

Then, added on top of this ConfigurationProvider, is fluidcontent's ConfigurationProvider which also triggers on the `tt_content`
table's records being manipulated but instead of "just" handling relations to other content elements, this ConfigurationProvider
also is capable of returning different template paths and template filenames based on the `tx_fed_fcefile` field of `tt_content`
(this field is named so for legacy reasons; it contains a specially formatted reference to the selected Fluid content type and the
collection to which it belongs).

These are just examples of how a ConfigurationProvider can be used in practice to control every little detail about how each Flux
template should be handled - and even which template should be used - based on the current record being rendered or manipulated.

The concept is also used in fluidpages where, instead of triggering on `tt_content` records, it triggers on `pages` records and
returns a template file, paths, variables etc. based on which page template is selected in the page properties (with inheritance
from parent pages supported as a native feature of fluidpages' ConfigurationProvider).

You can construct your own ConfigurationProvider classes for those cases when you need a greater degree of control over the Flux
form associated with, for example, your custom Extbase plugin.
