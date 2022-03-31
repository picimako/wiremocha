# WireMocha

[![Version](https://img.shields.io/jetbrains/plugin/v/18860-wiremocha.svg)](https://plugins.jetbrains.com/plugin/18860-wiremocha)
[![Downloads](https://img.shields.io/jetbrains/plugin/d/18860-wiremocha.svg)](https://plugins.jetbrains.com/plugin/18860-wiremocha)
![](https://img.shields.io/badge/since-IJ2021.3-blue) ![](https://img.shields.io/badge/until-IJ2022.1-blue)

<!-- Plugin description -->
WireMocha is an IntelliJ-based plugin that provides framework integration for the [WireMock](http://wiremock.org) library.

It offers various tools to generate and validate WireMock related test code, and to provide additional contextual information.
<!-- Plugin description end -->

## Purchase license

Since this plugin is available only via a paid license, you can head over to the [JetBrains Marketplace](https://plugins.jetbrains.com/plugin/18860-wiremocha) to get your hands on one.
You can find more information including prices and license types/schemes there.

## Dependencies

The WireMocha plugin depends on a few specific plugins whose absence alters the functionality of the plugin, and turns of the following features:

- **JUnit** (optional): JUnit Rule and extension field generation actions (see [JUnit Rules and Extensions](docs/junit_rules_and_extensions.md))
- **IntelliLang** (optional): various language injections (see [Language Injections](docs/language_injections.md))
- [**XPathView+XSLT**](https://plugins.jetbrains.com/plugin/12478-xpathview--xslt) (optional): XPath specific language injections
- [**Handlebars/Mustache**](https://plugins.jetbrains.com/plugin/6884-handlebars-mustache) (optional): Handlebars specific language injections

## Supported IDEs

The list of supported IDEs include all free versions of IntelliJ (Community, Educational, etc.) as well as Ultimate.

## Documentation

If you are interested in the **tl;dr** version of the plugin functionality, here are the main aspects that the plugin provides:

- Code generation for Junit 4 rules and JUnit 5 extensions, including conversion between the JUnit 5 programmatic and declarative approaches.
- Inspections for validating request matching and response definitions. Code folding for time based request matching, and conversion between `and()` and `or()` logical operators.
- Inspections and quick fixes to simplify response stubbing, as well as to identify and fix incorrect and duplicate response configurations.
- Intentions for inlining the contents of response body files, converting between ways of specifying them, and extracting body strings to files.
- Handlebars, regexp, XML and other language injections in Java and JSON DSLs.

Or, you can find more detailed documentation below:

- [Request Matching](docs/request_matching.md)
- [Live Templates](docs/live_templates.md)
- [Configuration](docs/configuration.md)
- [Response templating](docs/response_templating.md)
- [Stubbing](docs/stubbing.md)
- [JUnit Rules and Extensions](docs/junit_rules_and_extensions.md)
- [Language Injections](docs/language_injections.md)
