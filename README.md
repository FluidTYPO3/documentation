Fluid Powered TYPO3: Documentation
==================================

### Dear reader,

Thank you for your interest in the Fluid Powered TYPO3 project!

This documentation covers everything you could possibly wish to learn and know about templating with Fluid Powered TYPO3.

It is our hope that you find your new favorite templating workflow in this extension collection. We have tried to expect every possible need you might have and deliver ways to achieve your goals in less time than ever before. Our extensions do the boring work for you and enable you to focus fully on the templates themselves rather than how you should integrate them into TYPO3.

### Introduction

> TL;DR - in Fluid Powered TYPO3 template files are the main API and TypoScript setup only serves as a way to add or override template files and other resources, and to define basic constants-type settings which editors to change and which are used to modify the rendering of templates.

Before you begin, the most important fact you must know about Fluid Powered is this: **Fluid Powered TYPO3 is based on established conventions** coming from Extbase and Fluid, as well as TYPO3 CMS itself. The purpose of Fluid Powered TYPO3 extensions is **integration facilitation**. To illustrate this we can look at a page template written in Fluid - without the *integration facilitation* from `fluidpages` rendering a Fluid template as page template requires setting up a special TypoScript configuration and mapping (just an example) the backend or frontend layout selector field values to template files which can be rendered. But with the *integration facilitation* from `fluidpages`, using said page template becomes a simple matter of *placing it in a directory and adding this directory to the collection*.

The main difference between this approach and the (in TYPO3 CMS) perdominant approach, *configuration over convention*, is that instead of expecting a large configuration where you for example register all your new content element types and page template options as TypoScript options, Fluid Powered TYPO3 will instead expect a minimal configuration of *paths* rather than complete structures, and then detect and use templates in these paths. There are too many reasons why we chose this strategy to include them all in this introduction - you can read much more about each convention in the following chapters.

We also follow a practice of *including as (Fluid) XML in each template the options an editor can set to manipulate rendering*. This means that you would add a field which for example toggles display of the site footer in a page template, directly in the page template which renders said footer. Fluid Powered TYPO3 then reads this configuration directly from the template and renders it as fields which can be edited in the TYPO3 backend. We use this strategy in all contexts of Fluid Powered TYPO3 to *spare you from having to create additional database fields or configuration separately from your templates*. Generally speaking, you simply tell Fluid Powered TYPO3 where your template files are located and then start creating/using the templates without further configuration being needed.

There are, however, some key aspects where we diverge from the traditional practices of TYPO3 CMS. Being aware of these few simple ideas should keep any confusion to a minimum.

#### TypoScript is declarative

> TL;DR - in Fluid Powered TYPO3, every TypoScript setting is treated as declarative which means things like `stdWrap` and option splits are not applied to values.

In traditional TYPO3 CMS templating approaches there is a heavy use of so-called *TypoScript objects* which contain pseudo-instructions defining everything from the values of HTML tag attributes to conditions which render an element differently according to record values. In this traditional approach, integrators would affect the rendering of elements *by modifying the TypoScript rendering instructions*. In Fluid Powered TYPO3 we have slightly different requirements for integrators - namely that integrators must modify the options which editors have, the way elements are rendered and any conditions for how rendering is done, *by creating a new version of the template file in question and modifying the contents of said template*. Instead of creating a big collection of TypoScript value overrides and additions, an integrator simply adds a new template file location to the collection (overriding existing ones as needed) and then goes straight into the template to change every aspect of how it is rendered and edited. The replacement template is then used.

This approach means that *TypoScript is considered declarative only - In Fluid Powered TYPO3 it is used solely to configure constant-type settings*, the rest happens in each individual template, partial template or layout.

#### Automation expects convention

> TL;DR - in Fluid Powered TYPO3 you almost exclusively encounter `lowerCamelCase` and `UpperCamelCase` lettering with the exception of very few traditional contexts where `lowercase_underscored` is expected instead; namely in identifying extension keys.

Most of the features included in Fluid Powered TYPO3 in the shape of various TYPO3 extensions *require that conventions are followed*. This means that *you are more restricted in for example which names you can choose for your resources*. For example, *file names are expected to follow a particular `UpperCamelCase` format* (also called medial capitals or StudlyCaps) and *in almost all cases, `snake_case` formatting is not allowed*. This practice is different from traditional TypoScript- and TCA-centric customisations, but the consistency it imposes is the main reason why so much of the integration Fluid Powered TYPO3 enables can happen automatically. You can find detailed descriptions of these naming conventions in the following chapters.

No rule is without exception and this is also true in this case: there *are* circumstances where you still must use the traditional formats rather than the new ones, but it is always clearly indicated what is expected (for example: some ViewHelpers accept `extensionKey` parameters and these keys are still permitted to contain underscores, e.g. use `snake_case` naming). When this is the case it is clearly documented in the parameter description, ViewHelper attribute, PHP doc comment, etc.

### Summary

To recap the vital facts:

> Naming conventions are required for files and configuration options, TypoScript is regarded as declaration only and customisations are done by replacing (by way of configuration, not physically replacing) template files individually or in bulk.
