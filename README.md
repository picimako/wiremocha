# WireMocha plugin for IntelliJ

[![Version](https://img.shields.io/jetbrains/plugin/v/18860-wiremocha.svg)](https://plugins.jetbrains.com/plugin/18860-wiremocha)
[![Downloads](https://img.shields.io/jetbrains/plugin/d/18860-wiremocha.svg)](https://plugins.jetbrains.com/plugin/18860-wiremocha)
![](https://img.shields.io/badge/since-IJ2021.3-blue) ![](https://img.shields.io/badge/until-IJ2022.2-blue)

<!-- Plugin description -->
WireMocha is an IntelliJ-based plugin that provides framework integration for the [WireMock](http://wiremock.org) library.

It offers various tools to generate and validate WireMock related test code, and to provide additional contextual information, both in the JSON and Java DSLs.
<!-- Plugin description end -->

## Purchase license

Since this plugin is available only via a paid license, you can head over to the [JetBrains Marketplace](https://plugins.jetbrains.com/plugin/18860-wiremocha)
to get your hands on one. You can find more information including prices and license types/schemes there,
including a 30-day trial period.

## Dependencies

The WireMocha plugin depends on a few specific plugins whose absence alters the functionality of the plugin, and turns of the following features:

- **JUnit** (optional): JUnit Rule and extension field generation actions (see [JUnit Rules and Extensions](docs/junit_rules_and_extensions.md))
- **IntelliLang** (optional): various language injections (see [Language Injections](docs/language_injections.md))
- [**XPathView+XSLT**](https://plugins.jetbrains.com/plugin/12478-xpathview--xslt) (optional): XPath specific language injections
- [**Handlebars/Mustache**](https://plugins.jetbrains.com/plugin/6884-handlebars-mustache) (optional): Handlebars specific language injections

## Supported IDEs

The list of supported IDEs include all free versions of IntelliJ (Community, Educational, etc.) as well as Ultimate.

## Documentation

### tl;dr
- **JUnit support**: v4 rule and v5 extension code generation. Conversion between the v5 programmatic and declarative approaches.
- **JSON schema** based validation and code completion in mapping files.
- **Handlebars**, regexp, XML and other language injections in Java and JSON DSLs.
  - Inspections, code completion and references for Handlebars helpers.
- **Inspections and quick fixes**
  - to simplify response stubbing,
  - to identify and fix incorrect and duplicate response configurations,
  - for validating request matching and response definitions
- **Intentions** for inlining the contents of response body files, converting between ways of specifying them, and extracting body strings to files.

### Detailed
Or, you can find more detailed documentation below:

- [JUnit Rules and Extensions](docs/junit_rules_and_extensions.md)
- [Mapping files](docs/mapping_files.md)
- [Language Injections](docs/language_injections.md)
- [Response templating](docs/response_templating.md)
  - [Handlebars helpers](docs/handlebars_helpers.md)
- [Request Matching](docs/request_matching.md)
- [Stubbing](docs/stubbing.md)
- [Configuration](docs/configuration.md)
- [Live Templates](docs/live_templates.md)
- [Miscellaneous](docs/misc.md)
