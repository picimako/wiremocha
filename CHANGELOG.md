# WireMocha Changelog

## 1.0.8

### Added
- Added support for IntelliJ 2023.1.
- [34](https://github.com/picimako/wiremocha/issues/34): Added an intention action that can wrap a stub mapping JSON object in a single-mapping file
  wrap in the `mappings` property to prepare it for multiple mapping objects.
- [35](https://github.com/picimako/wiremocha/issues/35): Added a Project View context menu action to merge multiple stub mapping files into
  a newly created file.
- [39](https://github.com/picimako/wiremocha/issues/39): Added simple syntax highlighting for [JsonUnit placeholders](https://github.com/lukas-krecan/JsonUnit#typeplc)
  in Java files in the parameter of `com.github.tomakehurst.wiremock.client.ResponseDefinitionBuilder#withBody(java.lang.String)`.

## 1.0.7

### Fixed
- Fixed an exception during applying the following quick fixes:
  - the deletion of all Handlebars helper hashes when no hash is supported by that helper
  - when unwrapping single stub mappings in multi-mapping files regarding [33](https://github.com/picimako/wiremocha/issues/33)

## 1.0.6

### Added
- [33](https://github.com/picimako/wiremocha/issues/33): Added an inspection for JSON mapping files, that can unwrap single stub mappings wrapped in
  `mappings` properties.
- Added the `epoch` and `unix` code completion options to the `now` Handlebars helper's `format` hash.
- [35](https://github.com/picimako/wiremocha/issues/35): Add a context menu action to mappings directories (and their subdirectories)
  to merge all stub mappings files in the selected folder into a single file.
- [36](https://github.com/picimako/wiremocha/issues/36): Added a context menu action to JSON mapping files, that can split
  multi-mapping files into multiple single-mapping files.

### Changed
- Moved JSON mapping file related context menu actions into a submenu called *WireMock*.

### Fixed
- Fixed the classification of JSON multi-mapping files. In some cases it caused files to be shown as single-mapping ones in the Project View
  node decoration instead of multi-mapping.

## 1.0.5

### Added
- [28](https://github.com/picimako/wiremocha/issues/28): Added logic to detect duplicate request url definitions (`url`, `urlPattern`, `urlPath`, `urlPathPattern`) in mapping.json files.
- [29](https://github.com/picimako/wiremocha/issues/29): Added references to `response.bodyFileName` property values and
  `withBodyFile()` method call arguments, so that these file path references now show up in usage based searches.
- [30](https://github.com/picimako/wiremocha/issues/30): Added simple syntax highlighting for [JsonUnit placeholders](https://github.com/lukas-krecan/JsonUnit#typeplc)
  in JSON stub mapping files.
- [31](https://github.com/picimako/wiremocha/issues/31): Added a toolbar action to the Project View to show basic information of
  JSON stub mapping files. This information is displayed on the appropriate file nodes after the file name.

### Changed
- Code optimizations under the hood, in various areas, including stub simplification inspections.
- Renamed the stub mapping JSON schema to *WireMock Stub Mapping* to give it a more meaningful name.

## 1.0.4

### Added
- [24](https://github.com/picimako/wiremocha/issues/24): Added live templates for requests in the WireMock Admin API to be used
  with the [HTTP Client plugin](https://www.jetbrains.com/help/idea/http-client-in-product-code-editor.html).
- [25](https://github.com/picimako/wiremocha/issues/25): Added a WireMock tool window in which scenarios with their states can
  be modeled, and skeleton stub mappings can be generated from them.

## 1.0.3

### Added

- Added support for IntelliJ 2022.2.
- [6](https://github.com/picimako/wiremocha/issues/6): Added JSON schema for mapping files, which in turn provides validation and auto-completion
  of properties and values.
- [18](https://github.com/picimako/wiremocha/issues/18): Added extra validation for some helpers:
  reports `pickRandom` with a single-item collection, and reports `math` if an unsupported operation is specified
- [20](https://github.com/picimako/wiremocha/issues/20): Added quick fixes for Handlebars inspection to remove unsupported parameters and hashes,
  remove hashes specified with their default values, and to add missing mandatory hashes.
- [21](https://github.com/picimako/wiremocha/issues/21): Added code completion for the request attributes based on
  the [WireMock request model](https://wiremock.org/docs/response-templating/#the-request-model).

### Changed

- Remove **WireMocha** prefix from inspection messages.

## 1.0.2

### Added

- [11](https://github.com/picimako/wiremocha/issues/11), [12](https://github.com/picimako/wiremocha/issues/12):
  Extended inspection to detect duplicate request header and query param definitions in the Java DSL,
  within `com.github.tomakehurst.wiremock.client.MappingBuilder` and `com.github.tomakehurst.wiremock.matching.RequestPatternBuilder` call chains.
- [1](https://github.com/picimako/wiremocha/issues/1): Added an inspection that reports arguments of various method calls in builder classes
  that are equal to the property values that those methods configure.
- [4](https://github.com/picimako/wiremocha/issues/4): Extended inspection to detect `.withStatus()` response definition calls in chains in which 
  the first call already defines the status code. For example: `WireMock.ok().withStatus(301)`.
- [13](https://github.com/picimako/wiremocha/issues/13): Added an inspection that can validate various aspects of WireMock's custom Handlebars helpers. 
  For the full list of checks please check out the [Handlebars Helpers](/docs/handlebars_helpers.md) document.
- [14](https://github.com/picimako/wiremocha/issues/14): Added references to Handlebars helper names, so one can navigate to the implementation
  of custom helpers provided by WireMock, via Ctrl+Click. NOTE: only WireMock's helpers are supported, and they are not yet supported in subexpressions.
- [16](https://github.com/picimako/wiremocha/issues/16): Added code completion to Handlebars helper names, hashes and string literal hash values.
- [19](https://github.com/picimako/wiremocha/issues/19): Reworked Handlebars language injection, so that multiple Handlebars fragments are injected separately into the same
  string literal. This issue also adds support to inject Handlebars into string literals anywhere within the `response.headers` and `response.jsonBody` properties in the JSON DSL.

## 1.0.1

### Added

- [3](https://github.com/picimako/wiremocha/issues/3): A new quick fix is added to enable HTTPS in the `@WireMockTest` annotation if it is disabled either
  explicitly or by default, as in the `httpsEnabled` attribute is set to false.
- [2](https://github.com/picimako/wiremocha/issues/2): A new quick fix is also added to deduplicate `withHeader()` response definition calls for a selected
  header name.
- [7](https://github.com/picimako/wiremocha/issues/7): Added an inspection to detect duplicate response body definitions both in the Java and JSON DSLs, and a
  quick fix to deduplicate them keeping the one the user selects.
- [8](https://github.com/picimako/wiremocha/issues/8): Added various language injections into a Java and JSON DSLs.
- [5](https://github.com/picimako/wiremocha/issues/5): Added Handlebars language injection into methods of `ResponseDefinitionBuilder` in the Java DSL, and to a
  handful of JSON properties in mapping files.
- Response body line marker providers instead of showing a fix icon, now they show the icon of the file type the body path references.
- [9](https://github.com/picimako/wiremocha/issues/9): Added a context menu action to the Project view to create WireMock mapping file based on a predefined
  file templates. The action is available only on directories named `mappings` or directories under `mappings`.

## 1.0.0

### Added

- Initial set of features.
