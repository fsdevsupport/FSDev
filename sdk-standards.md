# FamilySearch SDK Standards

This document is a working specification for FamilySearch's SDK development and delivery across programming languages.

## Core Target Languages:

FamilySearch  targets the following core programming languages:

* PHP
* Java
* C#
* JavaScript

Future SDKs may include the following languages:

* Objective-C
* Python
* Ruby

## SDK Requirements

### Similar Interface
Each SDK should conform to an interface specification that makes the SDKs feel similar across languages making it possible for a developer to move from one language to another easily. This includes the following characteristics:

* Data Model - The class names and properties of the object model should be very similar across languages. This includes namespacing, and so forth.
* Client Interface - A naming convention should be established for API method calls. For example, the SDKs should be consistent with methods such as reading a person. Should the method name be "readPerson" or "getPerson" or just "person"?
* Convenience Methods - Similar convenience methods should be available across SDKs when working with the data model.

### Language Appropriate Design
Even though the SDKs should be similar across languages, it is understood that languages are different and have different conventions and features. The SDKs should be allowed to diverge to take advantage of language features. Here are a few examples of language appropriate divergence.

#### camelCase or underscore_case
A language like Java would expect methods and properties to be camelCased. A language like Ruby would expect to have method and property names underscore_cased.

#### Asynchronous Behavior
A language like JavaScript should expect API calls to happen asynchronously, therefore, it should support a model of callbacks or promises to handle this behavior. A language like PHP would not behave the same way, but would expect procedural execution.

#### Underlying Library Support
All languages have different libraries to support HTTP requests, document parsing, and so forth. This may cause some SDKs to diverge in available features.

For example, one HTTP library may support HTTP caching, where another language's library may not. The option to take advantage of a more advanced feature could be given to the SDK with better underlying library support.

### Minimum Language Version Support
Great care should be taken in choosing the minimum language version to support. Surveys to our developer community may be needed in order to determine the minimum version. This affects which libraries and language features can be used in the SDK. 

### Dependencies
Dependencies on third-party libraries should be chosen carefully. Decisions on dependencies should be well documented. The decision to include a third-party library dependency should weigh convenience against the cost of the dependency. It should evaluate the maturity of the library and the community support for the code. The following questions should be asked: 

* When was the library last updated?
* How thorough is the documentation?
* How many developers are using the library?
* How many dependencies are added by this library?

Wherever possible, dependencies should be managed through configuration files such as the following:

* PHP - composer.json (Composer)
* JavaScript - package.json (NPM/Node), bower.json (Bower)
* Java - pom.xml (Maven)
* C# - packages.config (NuGet)
* Ruby - Gemfile (Bundler)
* Python - setup.py (PyPi/pip)

When a dependency needs to be added solely for SDK development purposes, such as testing libraries, these should be marked as such and should not become a dependency for SDK end-users (developers).

The goal is to minimize the dependency footprint.

### Code Hosting
SDK code should be hosted on FamilySearch's Github organization. This will allow community developers to fork the SDKs and submit pull requests. The code should include a README.md file with instructions on how to setup the SDK development environment, to execute tests, and to build a project. It should also include instructions on how to contribute (fork, develop, test, and submit pull requests).

### Issue Tracking
SDK bugs and enhancements should be submitted and managed on the respective Github project Issue trackers.

### Continuous Integration
When possible, the code project should also tie into an automated build system such as [Travis-CI](https://travis-ci.org) and should feature a badge that shows the status of the build on the README page.

### SDK Distribution
Each SDK should be distributed through language appropriate distribution channels to support each language's preferred package manager. 

Some languages, like PHP, will also need a .zip download of the library that would include the required dependencies and installation instructions. This will facilitate the use of the SDK on hosting providers that don't support package management best practices.

### SDK Versioning
SDKs should be versioned using [semantic versioning](http://semver.org).

### SDK License
The SDK code should have a LICENSE file with an appropriate Open Source license document. The license should allow the SDKs to be used in closed commercial products. MIT and Apache 2 seem to be appropriate licenses.

## SDK Functionality

### Discovery and Hypermedia
Each SDK should use appropriate methods of discovery. Today, the appropriate method for discovery is the use of the collection resources. The SDKs should support the ability to move from one resource to another through hypermedia links on returned documents. The SDK should not force the developer to understand HATEOAS, but should allow the developer to use a HATEOAS interface if desired. 

The SDK should allow developers to jump directly to resources, given specific IDs, and should utilize URI templates to build the appropriate URIs for the calls.

### Rate Limiting
The SDKs should have a strategy for dealing with rate limiting (throttling). Perhaps an option could be available for the SDK to automatically wait and retry according to the *Retry-After* header value in the response. Another option is to raise an error and let the consumer of the SDK handle throttled responses in the app code.

### Setting Experiment Headers
The SDKs should have a mechanism that allows a developer to set experiment headers for testing pending modifications.

### Setting the User Agent
The SDKs should have the SDK library name in the User Agent (including dependent HTTP libraries). This should work according to the [HTTP 1.1 spec](http://tools.ietf.org/html/rfc7231#section-5.5.3).

Browsers have a limitation that does not allow the setting of a user agent on the XMLHttpRequest (XHR). 

### Logging
Each SDK should have a way to log the HTTP requests that are being made, including request headers and response headers. The log should be able to go to STDOUT, a file, or a central logging service.

Documentation should be provided on how to configure the SDK to log requests and responses.

### API Coverage
Each SDK should should strive to cover all major functions of the API. When new API features are released, or pending modifications are introduced, items should be added to the issue tracker in each project to ensure that the SDKs receive the maintenance needed to stay current with the API.

### Testing
Each SDK must have automated tests. Instructions must be included in the SDK README for setting up and running the tests. Pull requests from community members must also include automated tests and tests must pass. 


A strategy should be defined for automated test data. Currently, several approaches have been used to test SDKs including:

* Testing against API data that is stored locally within the project.
* Running tests against the FamilySearch Sandbox.
* Running tests against the FamilySearch Sandbox and caching results with a mechanism like Ruby's `vcr` gem.
* Creating mock data for each test.

Each approach has benefits and downsides. It would be beneficial to converge on a unified testing strategy across all SDKs.

## SDK Documentation

### SDK API Documentation
Each SDK should host a version of the SDK API documentation. This could be generated documentation based on the code and documentation within the code. All classes, public methods, and properties should be documented within the code. Example usage of the methods is helpful to be included in this documentation, but is not required.

The SDK API documentation should have a starting page that may be the same as the README.md documentation found on the homepage of the Github repository. This should include information on how to get started with the SDK.

### Document Getting Started with the SDK
Each SDK should have a robust getting started guide. This should include things such as environment prerequisites, installation instructions, configuration for authentication, and getting to a first hello world call.

### Example Project
Each SDK should have an example project that a developer can clone, configure, and run. These projects could be used to help a developer bootstrap a new project, or simply as a code reference that shows how all of the pieces come together.
