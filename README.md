# WireMocha plugin for IntelliJ

[![Version](https://img.shields.io/jetbrains/plugin/v/18860-wiremocha.svg)](https://plugins.jetbrains.com/plugin/18860-wiremocha)

<!-- Plugin description -->
WireMocha is an IntelliJ-based plugin that provides framework integration for the [WireMock](https://wiremock.org) library.

It offers various tools to generate and validate WireMock related test code, and to provide additional contextual information
in the following areas (the list is not comprehensive):
- JSON stub mapping files:
    - [JSON schemas](https://www.picimako.com/wiremocha/mapping-files/#1-json-schema) attached to stub mapping files for code completion and validation
    - [Handlebars helpers](https://www.picimako.com/wiremocha/response-templating-and-handlebars-helpers): validation, code completion and navigation
    - [Language injections](https://www.picimako.com/wiremocha/language-injections) in the JSON and Java DSLs
    - On-the-fly conversion of [JSON stub mapping files to the Java DSL](https://www.picimako.com/wiremocha/mapping-files/#18-preview-generated-java-code)
    - [Create, split and merge selected mapping objects and mapping files](https://www.picimako.com/wiremocha/mapping-files/)
    - [Request-response summary](https://www.picimako.com/wiremocha/mapping-files/#3-request-response-information-on-project-view-file-nodes) of mapping files in the Project View
- Java DSL:
    - [JUnit 4](https://www.picimako.com/wiremocha/junit-4-rules) and [JUnit 5](https://www.picimako.com/wiremocha/junit-5-extensions) support
    - [Testcontainers support](https://www.picimako.com/wiremocha/testcontainers)
    - Handlebars and other language injections
- Other features:
    - [WireMock tool window](https://www.picimako.com/wiremocha/wiremock-tool-window) with an Admin API client and a tool for Scenario generation
    - Various WireMock-specific static code analysis checks
    - Other features for code generation, optimization, navigation and more
<!-- Plugin description end -->

## Purchase license

The plugin is available via a paid license, and you can head over to the [JetBrains Marketplace](https://plugins.jetbrains.com/plugin/18860-wiremocha)
to get your hands on one. You can find more information about pricing and licensing options there, including a **14-day trial period**.

## Supported IDEs

The list of supported IDEs include all free versions of IntelliJ (Community, Educational, etc.) as well as Ultimate.

## Documentation

The documentation is available on its [dedicated website](https://www.picimako.com/wiremocha).

## OSS attributions

This project derives some built-in documentation and specifications from the [WireMock](https://github.com/wiremock/wiremock) open source project.
This content is licensed under the Apache License v2.
