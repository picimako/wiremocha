# WireMocha

![](https://img.shields.io/badge/since-IJ2021.3-blue) ![](https://img.shields.io/badge/until-IJ2022.1-blue)

<!-- Plugin description -->
WireMocha is an IntelliJ-based plugin that provides framework integration for the [WireMock](http://wiremock.org) library.

It offers various tools to generate and validate WireMock related test code, and to provide additional contextual information.
<!-- Plugin description end -->

## Purchase license

Since this plugin is available only via a paid license, you can head over to the JetBrains Marketplace to get your hands on one. You can find more information including prices and license types/schemes there.

## Dependencies

The WireMocha plugin depends on two specific plugins whose absence can alter the functionality of the plugin.

Although both the JUnit and the IntelliLang plugins are supposed to be bundled with the IDE, it can happen that they are missing or disabled.
On the JUnit plugin, JUnit Rule and extension field generation actions depend (see [JUnit Rules and Extensions](docs/junit_rules_and_extensions.md)), while 
on the IntelliLang plugin, various language injections depend (see [Language Injections](docs/language_injections.md)).

## Supported IDEs

The list of supported IDEs include all free versions of IntelliJ (Community, Educational, etc.) as well as Ultimate.

## Documentation

If you are interested in the tl;dr version of the functionality, here are the main aspects that the plugin provides:

- Code generation for Junit 4 rules and JUnit 5 extensions, including conversion between the JUnit 5 programmatic and declarative approaches.
- Inspections for validating request matching code, code folding for time based request matching, and conversion between `and()` and `or()` logical operators.
- Inspections and quick fixes to simplify response stubbing.
- Intentions for inlining the contents of response body files, converting between ways of specifying them, and extracting body strings to files.
- Regexp, XML and other language injections in Java code into various methods.

Or, you can find more detailed documentation below:

- [Request Matching](docs/request_matching.md)
- [Live Templates](docs/live_templates.md)
- [Configuration](docs/configuration.md)
- [Response templating](docs/response_templating.md)
- [Stubbing](docs/stubbing.md)
- [JUnit Rules and Extensions](docs/junit_rules_and_extensions.md)
- [Language Injections](docs/language_injections.md)
