Fluid Powered TYPO3: Documentation
==================================

> Welcome to the documentation collection for the extensions in the Fluid Powered TYPO3 family.

The documentation collection is divided into the following primary chapters:

* [Introduction](Introduction.md)
  Is a short description of what Fluid Powered TYPO3 is all about - in bullet point form.
* [Beginner's Guide](BeginnersGuide.md)
  Is a book-form guide which can be read from end to end. It teaches you the basic concepts shared by the extensions.
* [Best Practices](BestPractice/)
  Is a set of best practices for all areas involved in creating web sites based on Fluid TYPO3 extensions. It contains:
  * [Best practices for Configuration](BestPractice/Configuration.md)
  * [Best practices for Code Building](BestPractice/CodeBuilding.md)
  * [Best practices for Migrating Resources and Setups](BestPractice/Migration.md)
  * [Best practices for Content Template Creation](BestPractice/Content.md)
  * [Best practices for Page Template Creation](BestPractice/Pages.md)
  * [Best practices for Plugin Integration](BestPractice/Plugins.md)
* [Guides](Guides/)
  Is a set of guides for using the Flux API. It contains:
  * [Guide for using the FormComponent](Guides/FormComponent.md)
* [Extensions](Extensions.md)
  Is a more detailed set of documentation about each extension's features, requirements, practices etc.
* [Cookbook](Cookbook/)
  Is a collection of code examples.
* [Contributing](Contributing/)
  Is for those of you who wish to contribute code. It describes our workflow and quality expectations.

## Contributing to the documentation collection

> Wish to contribute? Have a correction? Great! :)

In order to contribute to the documentation you only need to respect a few easily remembered rules:

1. English is the only language allowed in this documentation - including cookbook recipes.
2. Respect the folder structure - don't add new folders, with one exception: in the cookbook folders are used to divide recipes
   into logical groups; if your recipe does not fit into any of the existing groups add one for it. When you do this you are
   encouraged to add multiple recipes for that group (in order to avoid groups with just one recipe). If your recipe fits into no
   group at all, use the `Miscellaneous` group along with a descriptive name for the recipe.
3. Try to use the same wording and sentence structure as the other documentation. Always use the same formatting! Lines are
   manually wrapped at 130 characters and documentation should be edited using a monospace font.
4. The official format is Markdown. Do not use other formats!
5. Make one change at a time. If necessary, create multiple changes (you can then submit them as one pull request).

You will most likely only be adding recipes to the cookbook and correcting the occasional typo, incorrect example or bad wording
in the main docs. A guide for writing good cookbook recipes can be found in [README.md of the Cookbook section](Cookbook/)

### Contribution how-to

Making changes and new files is easy - you can (like any other repository) clone the repository to your own account, make changes
as commits and then create pull requests.

The easiest way by far is to use the `Edit` button in the top right hand corner when viewing each file. When you click this button
the repository is forked to your account - if it has not already bene forked. You then make your changes and add a cover message;
when you submit this a so-called pull request is made and your change will be evaluated before being merged.

When writing your cover message please observe this single rule:

> The subject field (on Github, it has a placeholder saying `Update <filename>`) - please fill in this field with a text like
> `[DOC] Recipe for doing foobar in a baz template`. In other words: always write a subject and always begin it with `[DOC]` and
> begin the following sentence with a Capital letter. The `[DOC]` prefix is mandatory in this repository but note that other
> repositories also use other prefixes not described here. Make your subjects descriptive - the worst possible subject is one
> that just says `Updated <insert filename>` and says nothing about what was actually updated.
