Fluid Powered TYPO3: Concepts - Providers
=========================================

## Foreword

The Provider class is one of the more complex concepts used in Flux. It is opened up for "public" use since features such as
`fluidpages` and `fluidcontent` use this API themselves as a way to use advanced Flux features but do so through a limited API.
When you yourself use a Provider, it opens up ways to integrate with Flux which are similar to what `fluidcontent` does - based
on a particular type of record, making some aspects of rendering (fx which template file to use) dynamic based on values that are
set in the record.

### Disambiguation

The concept of [Provider Extensions](ProviderExtensions.md) (this document) should not be confused with the concept of
[Providers](Providers.md). The Provider extensions concept is about file structure, the Provider concept is about a type of class.

### API level 2

This concept is slightly more advanced than **layer 1** concepts; understanding it involves a basic PHP background knowledge and
some basic knowledge about Extbase extensions, making PHP classes and using a PHP class API in general (extending classes, calling
methods - along those lines).
