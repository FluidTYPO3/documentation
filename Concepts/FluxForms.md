Fluid Powered TYPO3: Concepts - Flux Forms
==========================================

## Foreword

This document describes in detail what the `Flux Form` concept means and how it works. The information is in-depth and should not
be used if you are starting out - if you are new to Flux, start with our [introduction](../Introduction.md), which introduces the
concepts in a much softer, more readily grasped format.

## What is the Form Component?

The Form Component in Flux is a set of classes which can be attached to each other in various ways, resulting in a combined
structure defining which fields and so on which the Flux Form should contain. One Form is, exactly like HTML forms, a group of
fields optionally grouped into `fieldset`s (which translates to Sheets in Flux).

As such, it is __the underlying structure objects of Flux's Form feature__. It and the [Provider concept](Providers.md) are the
two main new concepts introduced by Flux and together, they make up around 95% of Flux's feature set. The remaining 5% is a
special type of Extbase Controller which has a few additions to ease working with Flux's APIs, described in full detail in the
[Flux Controllers concept](FluxControllers.md) document.

The Flux API reuses almost all naming conventions from the TYPO3 core concept of TCEforms (known from TCA, for example) with a few
modifications (few as possible but still aiming to make the Flux API friendlier to read - for example by avoiding abbreviations).
In itself, the Flux Form Component _does not introduce any new functionality or features to the TCEforms concept in TYPO3 - it
"merely" makes it much more accessible and usable in other areas than just plain PHP arrays in TCA files_.

Flux's Form Component is, simply put, the fastest and most flexible way to create forms for the TYPO3 backend (and if plans go
the way they should it will even be possible to create frontend forms the same way some time in the near future).

With a few minor exceptions, the naming of settings you can use on Form instances is the same across all API flavors. Which means
that once you learn the basic idea of Flux Forms using the Form Component you can use this knowledge with all other flavors, too.

## The Benefits

What you gain from using the Flux Form Component API instead of pure PHP arrays is first and foremost a more IDE-friendly way of
creating forms: when you use the PHP API of Flux you have auto-completion of code and when you use the Fluid ViewHelper API you
have auto-completion as an optional feature, by using XSD schemas. There's a [great guide for using XSD schemas in PHPStorm](http://buzz.typo3.org/teams/extbase/article/howto-autocompletion-for-fluid-in-phpstorm/)
over at buzz.typo3.org - it uses Fluid Powered TYPO3's `schemaker` extension. The only API Flux provides which does not enable
any immediate auto-completion is the TypoScript API.

The Flux Form Component can be used to define Form structures for entire tables (effectively removing the need for TCA files) but
not only this: there is even an API flavor to use in Domain Model class files as annotations for the class and its properties,
letting you place the definition of the Form displayed when your Model's records are edited, right next to the Model properties.

## How does a Form Component structure look?

To briefly visualise the anatomy of a Flux Form:

```plain
Form
- name (fx "myform" or "tx_myext_domain_model_mytable")
- label (fx "My label" or NULL to trigger automatic labels)
- icon (fx "Resources/Public/Icons/myicon.gif
- Sheets (collection, Iterator)
  * Sheet one
  * Sheet two
    - name (fx "mysheet" or "options" - which by the way is the default sheet name)
    - label (fx "My sheet" or NULL for auto labels)
    - Fields (collection, Iterator)
      * Field one in Sheet two
        - name (fx "myfield")
        - label
        - type (fx "Input", "Checkbox" or "Select"
        - required (TRUE or FALSE)
        - Wizards (collection, Iterator)
          * Wizard one for Field one in Sheet two
            - type (fx "ColorPicker", "List", "Add", "Edit" etc)
```

As you can see from this structure some nodes are "Container nodes" which contain other nodes (example: Sheets can contain Field
nodes, Fields can contain Wizard nodes) and each node contains a few basic properties (there are many others; basically any Form
Component will require only the "name" attribute and allow you to use any number of additional properties as you please).

When this collection of objects which reflect a big tree (parent with N number of children which can each be parents) is handled
by Flux it is turned into TCEforms configuration arrays which you will know from the TCA concept in TYPO3. This configuration
array is then passed to TYPO3 in the method required to get the desired result (example: a FlexForm field or an entire TCA array;
naturally each of these targets imply slight differences in where the array is inserted and which nodes it should contain).

## Form Components' relation to Flux ViewHelpers

This part of the integration is actually extremely simple: just like the Form is itself a tree of objects (parents with children)
and the same is true for Fluid Templates (parent nodes with child nodes) a Form can very easily be defined as an XML node with
child nodes. And as current Flux users already will know: Flux has right from the start used this parent/child node relationship
in Fluid to allow - just as an example - the same Field Component type to be used both inside Sheets and Objects.

In other words: when you use a Flux ViewHelper, for example a form field, it is aware of the parent it belongs to and is turned
into a Form Component object instance which is then attached to the parent (inserted as the parent node in the Fluid template),
and so on, until the full tree structure is built.

Note: The only exception to the rule "Flux never renders any form fields" is here, in the "Custom" field type which lets you
define the actual HTML which will be inserted as field - it's not as such Flux which renders the field, but it is the exception
to the rule that everything is rendered by the TYPO3 core rather than Flux.

## Form Components' relation to the HTML form it defines

The actual display of Form Component fields added in Flux is handled entirely by the TYPO3 core (with one single exception, which
will be covered later) - put another way, Flux only ever _defines_ the form's structure, it never actually _renders_ any forms.

To customise the fields' display (for example, an input field's maximum characters or the options of a select field) you use the
built-in features of TYPO3: all the configuration options you know from TCA are available on Flux Form Components and can be set
using setter methods. Flux then delivers this special configuration to TYPO3 which renders the field in just the right way.

Flux should be seen as a basic _shell_ around TCA; it only allows the structures of TCA configuration to be created through an API
rather than as pure PHP arrays.

## The Flux API flavors

Flux provides four main flavors of the Form Component API: ViewHelpers for Fluid templates, PHP objects, TypoScript or Domain
Model class annotations. Each of these flavors has advantages and drawbacks - selecting the one you require is a vital task; once
the decision is made it is not easy to change to another flavor (e.g. switching from ViewHelpers to TypoScript - migration implies
completely rewriting the form). Some are limited to defining Forms to use as FlexForm replacements, others encompass table TCA.

### The ViewHelper API (original flavor)

This flavor of the API is ideal to use when your Flux form is tightly connected to a specific template file - which for example is
the case with `fluidpages` page template files or `fluidcontent` content element templates. This API is the original one provided
by Flux - it was since converted to a more generic pattern (but still preserving all features of course) which could more easily
be used outside Fluid templates.

Needless to say, this flavor of the API is only possible to use in Fluid templates.

Using this flavor of the API is as simple as using Fluid ViewHelpers in general. The API reference link below explains how to use
each individual component (as a ViewHelper) as well as gives you an idea about the structure you need to use.

* [Flux ViewHelper API reference](http://fedext.net/viewhelpers/flux.html)
* [Flux template structure reference](http://fedext.net/features/fluid-flexforms.html)

### The PHP API flavor

The second API flavor added to Flux is the PHP flavor. It works almost exactly like the ViewHelper API - the main difference is
that the PHP flavor naturally does not require a template file. Pro tip: this decoupling makes it excessively easy to reuse Form
objects - for example creating a basic instance and reusing it for multiple record types.

Being fully PHP based this flavor is of course the most flexible of all flavors; however, letting a site editor or integrator
actually configure Forms created in this flavor of the API obviously requires that you for example use TypoScript settings,
extension configuration options, configuration stored in records or something else altogether. Whereas in the Fluid and TypoScript
flavors you have the immediate option to use the special {settings} variable which gets filled with TypoScript settings for your
extension - and along with it (and highly recommended!) the option to use TypoScript constants which is a very integrator friendly
method of allowing configuration.

The PHP flavor is one of two flavors which can be used to define forms for full tables rather than individual fields on a specific
table (for example as done with the `pi_flexform` field on `tt_content` in the context of `fluidcontent`). The other flavor which
allows this is - quite logically - the Domain Model class annotation flavor.

**TODO: ADD API REFERENCE LINK**
**TODO: ADD COOKBOOK RECIPES LINK**

### The TypoScript API flavor

The third API flavor is the most basic one (being based on purely declarative TypoScript) but also the most integrator friendly
one. It is ideal to use only when your desired Flux Form meets these few conditions:

- Your Form does not need to be dynamic in any way
- Your Form should be extendable by other extensions (fx adding fields)
- Your Form should be easy for integrators to manipulate (fx disabling or renaming fields)

While the PHP flavor of course also supports this, it (in contrast to this flavor) requires a lot of manual effort to expose your
Form's structure to manipulation by integrators or allowing other extensions to for example add fields to the form. All of this
manipulation is much, much easier for integrators to do when Forms are entirely defined in TypoScript, the go-to declarative
options language in TYPO3.

However, the obvious drawback is the lack of dynamic processing - you can of course use basic TypoScript conditions and you do
get the added benefit of an _atomic configuration_ (e.g. one extension defines a form, another extension adds some fields and
an integrator then performs a third action) but due to the nature of TypoScript your choices are somewhat limited compared to fx
usage through PHP or Fluid ViewHelpers.

**TODO: ADD API REFERENCE LINK**
**TODO: ADD COOKBOOK RECIPES LINK**

### The Domain Model class annotation flavor

The fourth and final flavor is one based on so-called annotations - the special tags added to classes and class properties in
Extbase to define additional behavior, for example in Controller actions to define valiation behavior or in Model classes to
define Validation requirements for the Model. Just like you use annotations to specify how Extbase should validate instances of
your Model, you can use different annotations to define how TYPO3 (via Flux) should display form fields when records associated
with your Model are being edited in the backend.

Using this flavor is equal to manually creating a Form instance (PHP flavor) and associating it with a specific table name to
produce full TCA for the DB table name for your Model's records.

When you register a Domain Model class name in Flux, Flux will analyse the class and collect all the needed annotations and their
values from the class and property doc comments, then build a Form instance from this structure and use it as TCA.

You will still (at the time of writing this) be required to create the `ext_tables.sql` file with SQL schema definitions for your
Model's tables - however, efforts are being made to automate this process through the [builder](https://github.com/FluidTYPO3/builder)
extension also provided by the Fluid Powered TYPO3 team.

**TODO: ADD API REFERENCE LINK**
**TODO: ADD COOKBOOK RECIPES LINK**

### Mixing flavors

There is limited support for mixing flavors: by using a custom Provider (you can read more about this concept in the [Provider
Guide](Providers.md) you gain access using the PHP API flavor on Form instances created by using other flavors. In short: a
Provider class can implement a special method to post-process Form instances and creating a custom Provider gives you access to
this method which is the key to using the PHP flavor in post-processing for Forms from all other flavors.

However, the opposite direction is not possible (e.g. manipulating a PHP flavor Form from TypoScript) unless implemented manually
for example by letting your extension provide a few TypoScript options which your PHP side can then read and use when building
Form instances.

## Choosing the right flavor

It is of course important to make the right choice of flavor(s) for your needs. There are just a few things to consider when
choosing the flavor to use:

* For creating TCA you need the PHP and/or Model Object class annotation flavors.
* For dynamic forms it is recommended to use the Fluid or PHP flavors.
* For very simple forms or forms which must be possible to manipuate by integrators, use the TypoScript flavor.
* For specialised implementations - `fluidpages`, `fluidcontent`, `fluidbackend` and others - or implementations where your Form
  is tightly connected to a particular Fluid template - use the Fluid ViewHelper flavor.

### Shotgun

The "shotgun approach" to using the Flux Form Component is to use PHP. It allows you to create your Form for tables or individual
table fields, it allows dynamic manipulation (fx hiding form fields from non-admins) and it makes for somewhat easy integration
with integrator-friendly TypoScript settings at a later time. It makes for easy transition to using version control (if you don't
already do so but might need to in the future) since all the "business" of your Forms is stored in files rather than DB records.
And it's the fastest performing API flavor there is (simply because all other flavors are convert to this flavor before being
built and handed off to the TYPO3 core).

### Minimalist

The "minimalist approach" is to use pure TypoScript - it allows less advanced forms but is very easy to construct, it also
performs quite well (marginally slower than using pure PHP but much faster performance than the ViewHelper flavor).

### Specialist

The "specialist approach" is to use Fluid ViewHelpers - it's ideal for cases where one template has one form (best personified by
for example `fluidcontent` content element templates). It goes without saying that this flavor is the one most tightly connected
to the Fluid template which renders the data you save in the form - as such, it's the perfect choice for when your target is
an implementation based on individual template files (as opposed to for example full Extbase plugins which render multiple
templates but use the same form and form data for all of them - oldschool FlexForm style). However, in many cases it makes more
sense to use the PHP flavor unless you are required to use ViewHelpers (the way it is in `fluidcontent` for example). Creating a
custom implementation based on ViewHelpers is considerably more complex than using PHP or TypoScript - using the ViewHelpers is
easy but making your own connection between them and the TYPO3 core requires considerable knowledge of both Flux and TYPO3.
