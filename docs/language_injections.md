# Language Injections

Various [language injections](https://www.jetbrains.com/help/idea/using-language-injections.html#top) are provided by the WireMocha plugin that extend the
bundled configuration.

In general, injections require the **IntelliLang** plugin to be installed (bundled by default), while XPath injections require the **XPathView + XSLT** plugin,
and Handlebars injections require the **Handlebars/Mustache** plugin. All of these plugins are optional, but in case any of them is disabled or not installed,
some functionality of WireMocha won't be available.

As for the Handlebars injections: they don't provide advanced code insight like code completion, inspections, etc., for code within the injected fragments.

## Java

These are all enabled by default, and except the Handlebars injections, they can be disabled individually under <kbd>IDE Settings</kbd> > <kbd>Editor</kbd> > <kbd>Language Injections</kbd>.
They are by default available since v1.0.0, unless stated otherwise.

| Category                       | Location                                                                                                                                              | Injected Language | Comment                                            |
|--------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|----------------------------------------------------|
| WireMock URL Matchers          | `WireMock.urlMatching()`<br/>`WireMock.urlPathMatching()`                                                                                             | RegExp            |                                                    |
| WireMock String Value Matchers | `WireMock.matching()`<br/>`WireMock.notMatching()`                                                                                                    | RegExp            |                                                    |
| WireMock JSON Matchers         | `WireMock.equalToJson()`                                                                                                                              | JSON              |                                                    |
| WireMock XML Matchers          | `WireMock.equalToXml()`                                                                                                                               | XML               |                                                    |
| WireMock JSON Path Matchers    | `WireMock.matchingJsonPath()`                                                                                                                         | JSONPath          |                                                    |
| WireMock XPath Matchers        | `WireMock.matchingXPath()`<br/>`WireMock.matchesXPathWithSubMatcher()`                                                                                | XPath             |                                                    |
| WireMock JSON Response Body    | `WireMock.okJson()`<br/>`WireMock.jsonResponse()`                                                                                                     | JSON              |                                                    |
| WireMock XML Response Body     | `WireMock.okXml()`<br/>`WireMock.okTextXml()`                                                                                                         | XML               |                                                    |
| Response definition            | Following methods of `ResponseDefinitionBuilder`:<br/>`withBody()`<br/>`withBodyFile()`<br/>`withBase64Body()`<br/>`withHeader()`<br/>`proxiedFrom()` | Handlebars        | ![](https://img.shields.io/badge/since-1.0.1-blue) |

## JSON

Injections into JSON mapping files are generally available since v1.0.1.

At the moment, the injection into these properties cannot be disabled. 

| Property                                                                                                                                     | Injected Language |
|----------------------------------------------------------------------------------------------------------------------------------------------|-------------------|
| `matches`<br/>`doesNotMatch`<br/>`urlPattern`<br/>`urlPathPattern`<br/>`placeholderOpeningDelimiterRegex`<br/>`placeholderClosingDelimiterRegex` | RegExp            |
| `equalToJson`                                                                                                                                | JSON              |
| `equalToXml`                                                                                                                                 | XML               |
| `matchesJsonPath`<br/>`matchesJsonPath.expression`                                                                                                | JSONPath          |
| `matchesXPath`<br/>`matchesXPath.expression`                                                                                                      | XPath             |
| `body`<br/>`bodyFileName`<br/>`base64Body`<br/>`proxyBaseUrl`                                                                                | Handlebars        |
