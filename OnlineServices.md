Fluid Powered TYPO3: Online Services
====================================

A compiled list of services which you can use - resources for documentation, download locations, code generation services and such.

### Resources

1. [This documentation's repository on Github](https://github.com/FluidTYPO3/documentation). You can submit issues or request for
   changes to this documentation through the official repository.
2. [Official web site](https://fedext.net) contains blog/news, references and information about the project and team behind it.
3. [Support chat](https://fedext.net/support-chat.html) is an IRC plugin which lets you speak to us - when we are around.
4. [IRC channel #fedext](http://freenode.net/irc_servers.shtml) which you can access with any IRC client. It's the same channel
   you enter when you use the Support Chat.
5. [Continuous integration using Travis](https://travis-ci.org/FluidTYPO3) displays the current build status for all of our
   extensions. Here, you can investigate the logs in all glorious detail.
6. [ViewHelper Schema XSD files](https://fedext.net/viewhelpers.html) can be downloaded and used in your editor, for example
   [as explained for PHPstorm on buzz.typor.org](http://buzz.typo3.org/teams/extbase/article/howto-autocompletion-for-fluid-in-phpstorm/),
   to get autocompletion for all ViewHelpers in Fluid itself, and all Fluid Powered TYPO3 extensions.

### Services

There are a few services that you can use to get automatically generated packages for Fluid Powered TYPO3:

#### Provider Extension Generator

You can get a skeleton Provider Extension which uses the most current build function from [EXT:builder](https://github.com/FluidTYPO3/builder)
to generate on-the-fly a Provider Extension with your desired extension key.

To generate a Provider Extension, use this link - but replace `myextension` with the extension key you desire:

```
https://fedext.net/download/providerextension/myextension
```

A skeleton Provider Extension takes about a second to generate and is about 7KB in size. It will always be in `.zip` format which
you can either extract or upload to TYPO3 and install as extension.

#### Bootstrap Package

A complete TYPO3 site based on the "introduction" package from TYPO3 but using Twitter Bootstrap for page and content templates.
This package is built once in a while to contain the latest features for demonstration purposes.

You can download the most recent version at any time using these links:

* http://get.typo3.org/bootstrap (tar.gz)
* http://get.typo3.org/bootstrap/zip (zip)

The package archive is around 55MB in size and contains instructions for its installation.
