Fluid Powered TYPO3: Concepts - Controllers
===========================================

### API level 2

This concept is slightly more advanced than **layer 1** concepts; understanding it involves a basic PHP background knowledge and
some basic knowledge about Extbase extensions, making PHP classes and using a PHP class API in general (extending classes, calling
methods - along those lines).

### What is a Flux-enabled Controller?

There is not much difference between a standard Extbase plugin "Action Controller" and a Flux-enabled controller. The hint lies in
the name "-enabled" which means a _Flux-enabled controller is simply an extended controller with a few added behaviors_:

* A [Provider](Providers.md) is resolved by Flux and made available in the controller.
* The View class is different; allowing (ViewHelperVariableContainer-)variables to be read from specific sections.
* Several initialisation methods have been added, allowing for example common template variables to be added.
* Delegation to other controller classes is possible.

Other than these additions this type of controller class is a standard controller: it supports arguments, property mapping,
validation, redirection etc. just like all other controller types.

### Why use a Flux controller base?

You can opt to use a Flux-enabled controller whenever you want your controller to work more closely with your Provider class. Using
the controller base gives you automatic access to the Provider and lets the Provider deliver view variables and more. While you are
not required to use this controller base, it makes it easier to implement many view-related features. You can get the same result
by manually making your Provider instance and calling methods to read variables, template paths and other settings.
