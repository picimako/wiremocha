# Language Injections

IntelliJ can inject languages into string literals at pre-defined locations about which you can find some details in its [official documentation](https://www.jetbrains.com/help/idea/using-language-injections.html#top).

This configuration can be extended with configuration from plugins, thus various language injections are provided by the WireMocha plugin as well.
The locations and languages injected are listed down below.

These are all enabled by default, but they can anytime be disabled individually. Also, they are by default available since v0.1.0, unless stated otherwise.

| Category                       | Location                                                               | Injected Language |
|--------------------------------|------------------------------------------------------------------------|-------------------|
 | WireMock URL Matchers          | `WireMock.urlMatching()`<br/>`WireMock.urlPathMatching()`              | RegExp            |
| WireMock String Value Matchers | `WireMock.matching()`<br/>`WireMock.notMatching()`                     | RegExp            |
| WireMock JSON Matchers         | `WireMock.equalToJson()`                                               | JSON              |
| WireMock XML Matchers          | `WireMock.equalToXml()`                                                | XML               |
| WireMock JSON Path Matchers    | `WireMock.matchingJsonPath()`                                          | JSONPath          |
| WireMock XPath Matchers        | `WireMock.matchingXPath()`<br/>`WireMock.matchesXPathWithSubMatcher()` | XPath             |
| WireMock JSON Response Body    | `WireMock.okJson()`<br/>`WireMock.jsonResponse()`                      | JSON              |
| WireMock XML Response Body     | `WireMock.okXml()`<br/>`WireMock.okTextXml()`                          | XML               |
