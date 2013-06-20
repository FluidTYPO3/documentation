## Fluid Powered TYPO3: Best Practice for Code Building

This section of the documentation describes how to build code automatically. There are a few features in the Fluid Powered TYPO3
family which enable automatic code generation based on conventions. The first and most important such feature is the code
generation features of EXT:builder which can be found on Github: [https://github.com/FluidTYPO3/builder](https://github.com/FluidTYPO3/builder).

EXT:builder provides a few methods to validate and build code.

### Generating a Provider Extension

By running this CLI Command made available by EXT:builder, you can create a skeleton extension with a basic set of files and
configuration by using just a single command (from your TYPO3 document root):

```bash
./typo3/cli_dispatch.phpsh extbase builder:providerextension <arguments>
```

The command supports a wide range of arguments to affect which features the extension should use and how it should be built.

```plain
REQUIRED ARGUMENTS:
  --extension-key      The extension key which should be generated. Must not
                       exist in the typo3conf/ext folder.
  --author             The author of the extension, in the format "Name
                       Lastname <name@example.com>" with optional company name,
                       in which case form is "Name Lastname <name@example.com>,
                       Company Name"

OPTIONAL ARGUMENTS:
  --title              The title of the resulting extension, by default
                       "Provider extension for $enabledFeaturesList"
  --description        The description of the resulting extension, by default
                       "Provider extension for $enabledFeaturesList"
  --use-vhs            If TRUE, adds the VHS extension as dependency -
                       recommended, on by default
  --pages              If TRUE, generates basic files for implementing Fluid
                       Page templates
  --content            IF TRUE, generates basic files for implementing Fluid
                       Content templates
  --backend            If TRUE, generates basic files for implementing Fluid
                       Backend modules
  --controllers        If TRUE, generates controllers for each enabled feature.
                       Enabling $backend will always generate a controller
                       regardless of this toggle.
  --minimum-version    The minimum required core version for this extension,
                       defaults to latest LTS (currently 4.5)
  --dry                If TRUE, performs a dry run: does not write any files
                       but reports which files would have been written
  --verbose            If FALSE, suppresses a lot of the otherwise output
                       messages (to STDOUT)
  --git                If TRUE, initialises the newly created extension
                       directory as a Git repository and commits all files. You
                       can then "git add remote origin <URL>" and "git push
                       origin master -u" to push the initial state
  --travis             If TRUE, generates a Travis-CI build script which uses
                       Fluid Powered TYPO3 coding standards analysis and code
                       inspections to automate testing on Travis-CI
```

Depending on which toggles you use, template files will be built for EXT:fluidcontent, EXT:fluidpages and/or EXT:fluidbackend,
all containing a basic set of configuration enabling the file to be used by each extension. To make a "dry run" specify `--dry 1`
which means no files/folders are written but the intent to create each file/folder is output instead. You can even initialise
and initially commit all files as a git repository (requires the `git` CLI command to be available to the current shell user).

For example, such a build command might look like:

```shell
./typo3/cli_dispatch.phpsh extbase builder:providerextension test "Claus Due <claus@wildside.dk>" \
	--pages 1 --content 1 --controllers 1 --git 1 --travis 1 --use-vhs 1
```

Which would generate the extension key `test` authored by `Claus Due` with email `claus@wildside.dk` and include page and content
templates as well as controller classes for each, a Travis-CI build script to go along with the extension and finally will create
a git repository in the extension folder and initially commit all files.

You can then very easily install the generated extension also from the command line:

```shell
./typo3/cli_dispatch.phpsh extbase builder:install test
```

Which simply installs the extension key "test" (note: this feature only works on 6.0+ TYPO3 sites).

### Generating unit and functional tests

When you are "done" (if such a thing is possible) with your templates and classes related to your templates - Controllers,
ViewHelpers etc. - EXT:builder allows you to generate unit and functional tests for automateed testing of your code. There are
commands to generate ViewHelper class unit tests (for when you create custom ViewHelpers) and further test creation is planned
(for example: template validation based tests so phpunit will catch any Fluid syntax errors or incorrect arguments used).

There are even commands in EXT:builder to perform inspections of your templates, reporting any bad arguments or obvious syntax
problems (which you may not detect, for example if you change a custom ViewHelper's name or arguments and forget about a template
which uses the old names) and detecting problems in your PHP code. More analysis features are planned, for example ones to detect
problems in annotations used for example in Controllers, validating them to ensure required class types exist - and more.

### Using Travis-CI

[Travis CI](https://travis-ci.org/) is a service that is free to use for OpenSource projects - what it provides is a way to run
automated "builds" of your code, using scripts you can configure. EXT:builder can create a Travis build configuration which uses
Fluid Powered TYPO3 standards and code inspections to check your code. When built (and activated in Travis-CI by signing up and
switching on your repository in the list of your repositories), the Travis-CI script will automatically install dependencies as
required by your extension. EXT:builder is then used to perform a range of tests and finally, phpunit is called on to run any unit
tests your extension may contain (which includes tests built by using EXT:builder itself).

Whenever you receive pull requests or push to your master branch, Travis will execute the build script and report any problems to
you, the repository owner. [An example of a Travis build; for EXT:builder itself](https://travis-ci.org/FluidTYPO3/builder) and
[the script which configures that build](https://github.com/FluidTYPO3/builder/blob/master/.travis.yml).

Since even the most basic automated Travis-CI script written by EXT:builder will perform many tests on syntax in both PHP and
Fluid you are highly encouraged to use Travis-CI. But you can of course also perform the tests as needed, simply by running the
approproate CLI command from EXT:builder's list of commands.
