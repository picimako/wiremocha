# WireMocha

<!-- Plugin description -->
This IntelliJ-based plugin provides framework integration for the [WireMock](http://wiremock.org) library.

It offers various tools to generate and validate WireMock related test code, and to provide additional contextual information.
<!-- Plugin description end -->

## Dependencies

The WireMocha plugin depends on two specific plugins whose absence can alter the functionality of the plugin.

Although the both the JUnit and the IntelliLang plugins are supposed to be bundled with the IDE, it can happen that they are missing or disabled.
On the JUnit plugin, JUnit Rule and extension field generation actions depend (see [JUnit Rules and Extensions](docs/junit_rules_and_extensions.md)), while 
on the IntelliLang plugin, various language injections depend (see [Language Injections](docs/language_injections.md)).

## Supported IDEs

The list of supported IDEs include all free versions of IntelliJ (Community, Educational, etc.) as well as Ultimate, starting from version 2021.2 to 2022.1 as of now,
always supporting the last three major versions. Thus, once a new major version is released, along with adding support to the new one,
the plugin will drop support for the oldest supported major version of the IDE.

## Documentation

You can find detailed documentation in the dedicated documents below:

- [Request Matching](docs/request_matching.md)
- [Live Templates](docs/live_templates.md)
- [Configuration](docs/configuration.md)
- [Response templating](docs/response_templating.md)
- [Stubbing](docs/stubbing.md)
- [JUnit Rules and Extensions](docs/junit_rules_and_extensions.md)
- [Language Injections](docs/language_injections.md)

---
Plugin based on the [IntelliJ Platform Plugin Template][template].

[template]: https://github.com/JetBrains/intellij-platform-plugin-template
