# WireMocha Changelog

## 1.0.38

### Changed
- Supported IDE version range has changed to 2024.2.5 - 2025.1-EAP.
- Many refactorings and simplifications under the hood.
- Disabled a no longer valid WireMock Cloud API endpoint in the Admin API tool window.
- Raised the log level at one location regarding the Admin API tool window, so that the exception is displayed only in the
  IDE logs, but not on the IDE Internal Errors panel.

### Fixed
- Added the missing validation for the value of the `format` hash in the `formatJson` and `formatXml` Handlebars helpers.
- The Admin API tool window now again displays connection error and unknown host messages on the Admin root field when they occur.
- Handle `java.net.SocketException`s properly when sending requests from the Admin API tool window.

## 1.0.37

### Added
- [132](https://github.com/picimako/wiremocha/issues/132): Added support for the following Handlebars helpers: `formatJson`, `formatXml`, `jsonArrayAdd`, `jsonMerge`, `jsonRemove`, `toJson`.

### Changed
- The non-Handlebars language injections in JSON stub mapping files now use a simpler approach. In turn, they can no longer be enabled/disabled,
  these language injections are always enabled.
- Simplified the icon retrieval logic for wiremock-grpc-extension related line markers.
- Updated WireMock dependency version to 3.10.0.
- Some minor improvements under the hood.

### Fixed
- Handled a potential `NullPointerException` regarding Handlebars helper hash values.

## 1.0.36

### Added
- [126](https://github.com/picimako/wiremocha/issues/126): Added a new quick fix, so that `ok().withBody().withHeader()` call chains with specific **application/json**, **application/xml** and
  **text/xml** content type value can now be simplified directly as `okJson(body)`, `okXml(body)` and `okTextXml(body)`, respectively,
  instead of first replacing it with `okForContentType()`.

### Changed
- Updated the WireMock dependency to use v3.9.2.
- Added the `AbstractDateTimePattern#applyTruncationLast` property to the Java stub mapping preview.
- Response definition simplification inspections are now disabled inside the `com.github.tomakehurst.wiremock.client.WireMock` class
  because it caused false positives on the implementations of convenience methods this inspection is intended to simplify code to.
- [125](https://github.com/picimako/wiremocha/issues/125): CLI argument code completion items in `WireMockContainer#withCliArg()` are now displayed at the top of the list even when there
  are other, not necessarily relevant items (e.g. configuration properties) added by the IDE itself.

### Fixed
- Fixed an exception that occurred during handling Handlebars language injection into invalid JSON code elements.
- Fixed an `InvalidVirtualFileAccessException` that occurred when the plugin attempted to inject Handlebars language
  into files that no longer existed, e.g. when splitting multi-mapping files.
- Fixed the `WebhookDefinition` JSON generation when converting a StubMapping in the Java DSL to an HTTP Client request.
- Added the missing import statement for `se.bjurr.wiremockpact.wiremockpactlib.api.WireMockPactConfig` in the WireMockPactExtension
  code template.
- JSON request bodies of HTTP Client requests generated in the Admin API tool window now have proper indentation.

## 1.0.35

### Changed
- [118](https://github.com/picimako/wiremocha/issues/118): Project view node decoration of custom stub mapping root directories now supports subprojects of workspaces
  created by the Multi-Project Workspace plugin.
- [118](https://github.com/picimako/wiremocha/issues/118): Workspaces are now supported during the lookup of `__files` directories when extracting response body strings to file
  from the Java DSL, or inlining them from response body files.
- [123](https://github.com/picimako/wiremocha/issues/123): Variable and field resolution are now also supported (to a certain extent) when generating HTTP Client requests from
  Java DSL calls to Admin API endpoints.

### Fixed
- [121](https://github.com/picimako/wiremocha/issues/121): Values in `request.url` and `request.urlPath` JSON properties, as well as `urlEqualTo()` and `urlPathEqualTo()` calls are
  no longer reported as potential regex patterns when they contain dot (.) symbols.
- `Webhooks.webhook()` call chains without URLs specified are no longer reported when used outside `withServeEventListener()` and `withPostServeAction()` calls,
  because the URL may be configured elsewhere in them.

## 1.0.34

### Added
- Added new Admin API + HTTP Client specific live templates for file, settings, version and health related endpoints.
- [61](https://github.com/picimako/wiremocha/issues/61): Extended the 'Open in HTTP Client' line marker icon to support the majority of Admin API Java calls (where feasible)
  and generate path parameters and request body JSONs dynamically based on the configuration of those Java method calls.
- Added the *Delete matching stub mapping (`DELETE /__admin/mappings/remove`)* endpoint to the Admin API tool window.

### Changed
- Supported IDE version range has changed to 2024.1.6 - 2024.3.*.

### Fixed
- Declarative <-> programmatic JUnit 5 related intentions are no longer available in projects that don't use WireMock.
- Fixed the names of a few Admin API + HTTP Client specific live templates.
- Fixed a `NoSuchElementException` that occurred when getting the last element of an empty collection during webhook configuration checks
  in the Java DSL.
- Fixed a `NullPointerException` that occurred when opening the plugin settings page.
- [122](https://github.com/picimako/wiremocha/issues/122): Fixed the error message when reporting less than required argument count for `WireMock.and()` and `WireMock.or()`.
- [122](https://github.com/picimako/wiremocha/issues/122): `WireMock.and()` and `WireMock.or()` are no longer reported with one argument when that argument's type is `StringValuePattern[]`.

## 1.0.33

### Fixed
- [106](https://github.com/picimako/wiremocha/issues/106): Fixed exceptions that occurred during deleting helper hashes from injected Handlebars fragments.
- [106](https://github.com/picimako/wiremocha/issues/106): Fixed an exception regarding Handlebars language injection when deleting JSON properties from stub mapping files.

## 1.0.32

### Added
- [111](https://github.com/picimako/wiremocha/issues/111): Added a new option on the Admin API client GUI that can generate a request in the Http Client format
  of the currently configured request, and copy it to the clipboard.
- [116](https://github.com/picimako/wiremocha/issues/116): Added the *Reload gRPC descriptor files* endpoint from the wiremock-grpc-extension to the Admin API tool window.

### Fixed
- Fixed a `NullPointerException` regarding mapping file node decoration in the Project View.
- The success message of the **Remove requests by criteria** Admin API endpoint now uses the correct message key.

## 1.0.31

### Added
- [114](https://github.com/picimako/wiremocha/issues/114): Added the `count` hash for the **pickRandom** Handlebars helper.

### Changed
- Moved a few hardcoded string literals to a property bundle to enable their potential future translation.
- Various minor maintenance changes.

## 1.0.30

### Added
- [110](https://github.com/picimako/wiremocha/issues/110): Added proxy and authentication configuration for the Admin API tool window.
- [108](https://github.com/picimako/wiremocha/issues/108): Added support for the **Get all file names**, **Delete file by ID**, **Return the version of the WireMock server**,
  **Return the health of the WireMock server** and **Get global settings** Admin API endpoints.
- [112](https://github.com/picimako/wiremocha/issues/112): Added support for WireMock Cloud's Provisioning API in the WireMock tool window.

### Changed
- A few more exception types are handled that may occur when targeting endpoints on WireMock Cloud or other remote servers.
- [105](https://github.com/picimako/wiremocha/issues/105): Many actions, inspections, intention actions and more are no longer considered for execution in projects
  that don't use WireMock.

### Fixed
- [109](https://github.com/picimako/wiremocha/issues/109): Code completion of Handlebars hashes and request attributes are again available in mapping files outside content roots.

## 1.0.29

### Added
- [88](https://github.com/picimako/wiremocha/issues/88): Added an Admin API client to the WireMock tool window, so that users are able to work with externally started
standalone WireMock servers.
- Added JSON language injection into `com.github.tomakehurst.wiremock.stubbing.StubMapping.buildFrom(String)`.
- [107](https://github.com/picimako/wiremocha/issues/107): Added support for the `ignoreOrderOfSameNode` attribute of `EqualToXmlPattern` in the Java preview and the various stub mapping schemas.
- [107](https://github.com/picimako/wiremocha/issues/107): Added the `id`, `bodyAsBase64`, `multipart` and `parts` attributes to the `request` and `originalRequest` model code completion.
- [107](https://github.com/picimako/wiremocha/issues/107): Added default method argument validation for `preserveUserAgentProxyHeader()`, `disableConnectionReuse()`,
`withMaxTemplateCacheEntries()` and `maxHttpClientConnections()` in `WireMockConfiguration`.
- [107](https://github.com/picimako/wiremocha/issues/107): Added the `max-proxy-http-client-connections, `disable-proxy-client-connection-reuse, `max-http-client-connections`,
and `disable-connection-reuse` CLI options to the Testcontainers code completion.
- [107](https://github.com/picimako/wiremocha/issues/107): Added an inlay hint for `WireMockConfiguration.withMaxTemplateCacheEntries()` when passed in `null`
to signal that it sets a no-limit cache.

### Fixed
- Added missing `StringValuePattern` options to the stub mapping schema when completing `MultiValuePattern` options.

## 1.0.28

### Added
- [104](https://github.com/picimako/wiremocha/issues/104): Added inspection, inlay hint, reference, etc. support for the `arrayJoin`, `arrayAdd`, `arrayRemove` and `val` Handlebars helpers.
- [104](https://github.com/picimako/wiremocha/issues/104): Made Handlebars helper name completion version specific, so the e.g. `arrayJoin` is not code completed when using WireMock earlier than 3.6.0.
- [104](https://github.com/picimako/wiremocha/issues/104): Added `supported-proxy-encodings` to the code completion of command line arguments in Testcontainers code.
- [103](https://github.com/picimako/wiremocha/issues/103): Added a quick documentation popup for the hash parameters of WireMock's Handlebars helpers.

### Changed
- Supported IDE version range has changed to 2023.3 - 2024.2-EAP.
- Renamed the latest stub mapping schema from *WireMock 3.5.0 Stub Mapping* to *WireMock 3.5.0+ Stub Mapping*,
  so that it is clearer that it is not only for version 3.5.0.
- Hash name code completion now inserts an = symbol as well, right after the hash name, for smoother coding experience.
- Improved the re-injection logic of Handlebars fragments to cover more cases when a JSON stub mapping files is changed.
- Inject/uninject languages in open files upon enabling/disabling the corresponding WireMocha plugin settings.
- Some wiremock-grpc-extension related logic is no longer enabled when the extension is not available in the project,
  to do a small performance improvement.

### Fixed
- Eliminated *Slow operations are prohibited on EDT* errors regarding Handlebars language injections in stub mapping files.
- Inject Handlebars fragments in JSON stub mapping files on project startup, only when Handlebars injection is enabled
  in the plugin settings.
- Start injecting Handlebars fragments in JSON stub mapping files when Handlebars injection gets enabled
  in the plugin settings, and it wasn't before.

## 1.0.27

### Added
- Added code completion for `StringValuePattern` properties inside the `request.pathParameters` and `request.cookies` properties.
- Added code completion of `MultiValuePattern` properties inside `request.queryParameters`, `request.formParameters`, `request.headers`
  and `request.multipartPatterns.headers`.
- [100](https://github.com/picimako/wiremocha/issues/100): Added descriptions to code completion elements when completing
  Handlebars helper hashes. An icon for mandatory hashes is also added.
- [94](https://github.com/picimako/wiremocha/issues/94): Added default method argument value reporting for the methods in `se.bjurr.wiremockpact.wiremockpactlib.api.WireMockPactConfig`.
- [94](https://github.com/picimako/wiremocha/issues/94): Added Regexp language injection to arguments of some methods in `se.bjurr.wiremockpact.wiremockpactlib.api.WireMockPactConfig`.
- [94](https://github.com/picimako/wiremocha/issues/94): Convert `WireMockPactConfig` metadata from JSON to Java in the stub mapping preview.
- [94](https://github.com/picimako/wiremocha/issues/94): Added an action into the Alt+Insert generate menu that can generate a `WireMockPactExtension`-type JUnit5 `@RegisterExtension` field.

### Changed
- Minor improvements on stub-mapping JSON property descriptions.
- Changed the stub mapping Java preview editor's name to *Raw / Java Preview*, so that it fits better with the WireMock plugin's *Data* tab.

### Fixed
- Fixed an exception that occurred during previewing the results of the *Remove redundant method call* intention action.
- Fixed an unhandled `JsonSyntaxException` during the Java preview of JSON stub mapping files.

## 1.0.26

### Added
- [97](https://github.com/picimako/wiremocha/issues/97): Added support for the `jwt` and `jwks` Handlebars helpers provided
  by the wiremock-jwt-extension.
- [99](https://github.com/picimako/wiremocha/issues/99): The JSON schema provided by WireMocha is now assigned to the mapping files in the *WireMock Stubs*
  scratch folder that is used by the JetBrains WireMock plugin to generate mappings from endpoints.
- [98](https://github.com/picimako/wiremocha/issues/98): Improved the stub mapping JSON schema: delay distributions got additional validation for the required
  properties. StringValuePatterns are also code completed in the `request.host`, `request.bodyPatterns` and `request.multipartPattern.bodyPatterns` properties.

## 1.0.25

### Added
- [95](https://github.com/picimako/wiremocha/issues/95): Added code completion for the new CLI arguments: disable-http2-plain, disable-http2-tls.
- [95](https://github.com/picimako/wiremocha/issues/95): Added code completion for the `default` hash of the `systemValue` Handlebars helper.
- [95](https://github.com/picimako/wiremocha/issues/95): Added a WireMock 3.5.0 specific stub mapping schema that contains the `response.removeProxyRequestHeaders` property.
- [95](https://github.com/picimako/wiremocha/issues/95): The stub mapping Java preview now converts the `response.removeProxyRequestHeaders` property as well.
- [91](https://github.com/picimako/wiremocha/issues/91): Added a project-level toggle option in the plugin settings, so that users can opt in to use the stub mapping schema provided
by JetBrains' WireMock plugin. By default, the ones provided by WireMocha are applied.
- [96](https://github.com/picimako/wiremocha/issues/96): Extended the code completion of Data Faker providers with the Food, Sport,
Videogame and Entertainment groups.

### Changed
- [93](https://github.com/picimako/wiremocha/issues/93): Improved cache refreshing conditions around stub mapping file retrieval.
- Improved the JSON schema assignment logic for stub mapping files.
- [92](https://github.com/picimako/wiremocha/issues/92): Changed the GRPC service line marker icon to a Protocol Buffer specific one.

### Fixed
- JSON schemas that are not applicable to an edited stub mapping file are no longer displayed in the schema selector at the bottom of the IDE.
- Fixed an exception that could occur when files were being closed during project initialization.

## 1.0.24

### Added
- [90](https://github.com/picimako/wiremocha/issues/90) - **wiremock-grpc-extension**: Added Handlebars language injection into `WireMockGrpc.jsonTemplate()`, and JSON language injection into `WireMockGrpc.json()`.
- [90](https://github.com/picimako/wiremocha/issues/90) - **wiremock-grpc-extension**: Added code completion of `WireMockGrpc.Status` constants in the `response.headers.grpc-status-name` property in the JSON DSL.
- [90](https://github.com/picimako/wiremocha/issues/90) - **wiremock-grpc-extension**: Added an inspection to report invalid `WireMockGrpc.Status` values in the `response.headers.grpc-status-name` property in the JSON DSL.
- [90](https://github.com/picimako/wiremocha/issues/90) - **wiremock-grpc-extension**: Added an inspection to report references to potential GRPC service calls specified in `request.url*` (except `request.urlPath`) JSON properties.
- [90](https://github.com/picimako/wiremocha/issues/90) - **wiremock-grpc-extension**: Added an inspection to report GRPC service references in `request.urlPath` when not used with POST request method.
- [90](https://github.com/picimako/wiremocha/issues/90) - **wiremock-grpc-extension**: Added line markers to `request.urlPath` JSON properties to open the corresponding service implementation Java class or service declaration in a `.proto` file.
- [90](https://github.com/picimako/wiremocha/issues/90) - **wiremock-grpc-extension**: Added code completion of GRPC service methods in the `request.urlPath` JSON property.

## 1.0.23

### Added
- [87](https://github.com/picimako/wiremocha/issues/87) - **wiremock-faker-extension**: Added code completion for Data Faker providers in the `random` Handlebars helper.
- [89](https://github.com/picimako/wiremocha/issues/89): Added code completion for serve event listeners in JSON stub mapping files.
  Currently `webhook` as well as listeners from the wiremock-state-extension are supported.
- [68](https://github.com/picimako/wiremocha/issues/68) - **wiremock-state-extension**: Added inspections, code completion and references for the `state` Handlebars helper.
- [68](https://github.com/picimako/wiremocha/issues/68) - **wiremock-state-extension**: Added inspections to report configuration issues with the `recordState` and `deleteState` serve event listener.
- [68](https://github.com/picimako/wiremocha/issues/68) - **wiremock-state-extension**: Handlebars language is now injected into JSON string literal properties under the `recordState` and `deleteState` serve event listener,
  and for properties under the `request.customMatcher` for the `state-matcher`.
- [68](https://github.com/picimako/wiremocha/issues/68) - **wiremock-state-extension**: Added inspections to report configuration issues with the `state-matcher` custom request matcher.

### Changed
- The configuration of the `random` and `state` helpers are now validated only when the wiremock-faker-extension and wiremock-state extension are
  available in the project, respectively. This prevents reporting false positives when one uses helpers with the same names but from
  extensions not supported by WireMocha.

## 1.0.22

### Added
- [84](https://github.com/picimako/wiremocha/issues/84): Added code completion for JsonUnit placeholders in Java and JSON string literals.
- [84](https://github.com/picimako/wiremocha/issues/84): Added code completion for XmlUnit placeholders in Java and JSON string literals, as well as XML tags and XML attribute values.
- Added syntax highlighting for JsonUnit placeholders starting with a #, like `#{json-unit....}`. Before, only the `${json-unit....}` format was supported.
- Added syntax highlighting for XmlUnit placeholders in Java and JSON string literals.
- Added plugin settings to toggle JsonUnit and XmlUnit placeholder highlighting. They can be enabled/disabled separately.
- [85](https://github.com/picimako/wiremocha/issues/85): Added an inspection to report issues regarding the `enablePlaceholders` configuration of `EqualToXmlPattern` in JSON stub mapping and Java files.
- [81](https://github.com/picimako/wiremocha/issues/81): The extension [RandomHelper](https://github.com/wiremock/wiremock-faker-extension) is now recognized by the plugin and inspections and references are applied.
- [86](https://github.com/picimako/wiremocha/issues/86) - **WireMock 3.4.0**: Added `--version` to list of CLI options when code completing options in Testcontainers Java configuration.
- [86](https://github.com/picimako/wiremocha/issues/86) - **WireMock 3.4.0**: Added the `extensionScanningEnabled` configuration option when converting JUnit5 test classes from the declarative to programmatic approach and vice versa.
- [86](https://github.com/picimako/wiremocha/issues/86) - **WireMock 3.4.0**: Added the `GET_OR_HEAD` request method to the stub mapping preview and the scenario generation tool window.

### Changed
- Supported IDE version range has changed to 2023.2 - 2024.1-EAP.
- [82](https://github.com/picimako/wiremocha/issues/82): The Handlebars language is now injected into complete Java and JSON string literals instead of injecting fragments into
  the helper and similar expression parts of them.
- Simplified and optimized the JsonUnit placeholder annotation logic for the Java and JSON DSLs. In case of the Java DSL, the restriction that the annotated String literal had
  to be in the argument list of `ResponseDefinitionBuilder#withBody(String)` has been lifted.
- JsonPath language injection is now re-enabled both for JSON and Java files.
- [86](https://github.com/picimako/wiremocha/issues/86) - **WireMock 3.4.0**: The intention to convert JUnit 5 classes from programmatic to declarative now checks for the absence of the `@WireMockTest`
  annotation on base classes as well.

### Fixed
- [82](https://github.com/picimako/wiremocha/issues/82): The Handlebars language is now injected properly into Java text blocks.
- Handlebars code completions are now available again in Java and JSON String literals injected with Handlebars language.
- JsonUnit placeholders are now annotated in Java String literals inside String concatenations with +.
- The **Merge Selected Mapping Files** action is available again on selected JSON stub mapping files.
- Fixed a few instances of the same exception regarding action updates.
- Fixed a plugin settings option title.
- Fixed the scenario generation tool window to update generated code snippets upon deleting rows from the table.

## 1.0.21

### Changed
- Made a small modification in the inspection message related to invalid date-time truncations in the `truncateDate` helper.

### Fixed
- Fixed an exception thrown when the `truncateDate` Handlebars helpers was supplied with an invalidate date-time truncation value.

## 1.0.20

### Added
- [77](https://github.com/picimako/wiremocha/issues/77): Added an inspection to report invalid date-time truncation values in the `truncateDate` Handlebars helper.
- [77](https://github.com/picimako/wiremocha/issues/77): Added code completion of date-time truncation values for the truncation parameter of the `truncateDate` Handlebars helper.
- [77](https://github.com/picimako/wiremocha/issues/77): Added reference to the date-time truncation values for the truncation parameter, so that users can navigate to the corresponding `DateTimeTruncation` enum constants
  with Ctrl+Click and similar mechanisms.

### Changed
- Added the WireMock version into the JSON stub mapping schema names to be able to distinguish them in the JSON schema selector menu.
- [79](https://github.com/picimako/wiremocha/issues/79): Moved the plugin settings under the *Language & Frameworks* settings page.
- [80](https://github.com/picimako/wiremocha/issues/80): Reorganized inspections and intention actions in the IDE settings,
  into DSL and language specific groups. This is also a preparation for introducing support for the coming YAML DSL.
- Code optimizations under the hood.

### Fixed
- Fixed the problem that one of the intention actions for generating path parameter matchers from a URL path template was not available in the IDE settings
  due to both the Java and JSON intention having had the same name.
- Fixed the problem that when multiple Handlebars fragments were injected into the same Java or JSON String literal, starting from the second helper,
  navigation to the WireMock helper implementation via Ctrl+click didn't work. It worked only for the first helper name in the string.

## 1.0.19

### Fixed
- [78](https://github.com/picimako/wiremocha/issues/78): Fixed the issue that no WireMock schema was attached to JSON stub mapping files located in modules without any WireMock version configured.

## 1.0.18

### Added
- [54](https://github.com/picimako/wiremocha/issues/54): Custom mapping root dirs can now be specified in the plugin settings. For now, the set
  root dirs are displayed with a dedicated icon in the Project View.
- [72](https://github.com/picimako/wiremocha/issues/72): Added opt-in inlay hints to Handlebars helper parameters in fragments injected into JSON stub mapping files.
  Besides the default option to toggle the inlay hints themselves, it also provides an option to toggle the hints for sub-expressions separately.
- [76](https://github.com/picimako/wiremocha/issues/76): Added plugin settings to be able to toggle Handlebars language injection in JSON stub mapping files,
  and Java DSL methods, as well as to toggle other language injections in JSON stub mapping files.
- [75](https://github.com/picimako/wiremocha/issues/75): In the New Directory creation popup, completion items for `mappings` and `__files` directories are displayed to speed up the creation
  of them.
- Added 'x' as a valid mathematical operator to the `math` Handlebars helper validation.

### Changed
- Supported IDE version range has changed to 2023.1.1 - 2023.3-Beta.
- Improved the inspection messages of Handlebars helper inspections regarding validating the number of specified parameters. From now on, where feasible,
  it includes a short description of the missing parameters, instead of just the overall mandatory number of parameters.
- Various optimizations under the hood.

### Fixed
- Fixed an exception thrown when deleting a Handlebars helper hash via quick fix.
- Fixed an exception thrown when adding missing mandatory hashes to a Handlebars helper via quick fix.

## 1.0.17

### Added
- [70](https://github.com/picimako/wiremocha/issues/70): Added RegExp language injection into `WireMockConfiguration#withPermittedSystemKeys`.

### Changed
- Dependency version updates.
- Various code optimizations.

### Fixed
- Some minor bugfixes.

## 1.0.16

### Added
- **WireMock 3.2.0**: Added the `disable-response-templating` CLI option to the list of code completion items for Testcontainers configuration.
- **WireMock 3.2.0**: Added the default argument value analysis for `WireMockConfiguration.extensionScanningEnabled()`,
  and the same CLI option, `disable-extensions-scanning`, to Testcontainers code completion.
- [69](https://github.com/picimako/wiremocha/issues/69): Added code completion of Handlebars helper request attributes for the `originalRequest` property based on the
  [Simulating Webhooks and Callbacks](https://wiremock.org/3.x/docs/webhooks-and-callbacks/#using-data-from-the-original-request) WireMock documentation.
- [69](https://github.com/picimako/wiremocha/issues/69): Added Handlebars language injection into `serveEventListeners.parameters.url/method/body` properties,
  when the listener name is *webhook*, and into the `withUrl()`, `withMethod()` and `withBody()` methods of `WebhookDefinition`.
- [69](https://github.com/picimako/wiremocha/issues/69): Added Content-Type header value code completion for parameters of `WebhookDefinition.withHeader()`,
  and in the `serveEventListeners.parameters.headers` JSON property.
- [69](https://github.com/picimako/wiremocha/issues/69): Webhooks as serve event listeners (WireMock 3.x) are now converted as specific webhooks
  from JSON to Java in the stub-mapping preview.
- [69](https://github.com/picimako/wiremocha/issues/69): Added a handful of static code analysis checks for `Webhooks.webhook()` call chains and webhook specific `serveEventListeners.parameters` properties.
- Added the `BEFORE_RESPONSE_SENT` request phase to the stub-mapping JSON schema for [WireMock/2294](https://github.com/wiremock/wiremock/issues/2294).

### Changed
- Supported IDE version range has changed to 2022.2 - 2023.3-EAP.
- Optimized the insertion of `WireMockRule` and `WireMockExtension` fields in JUnit test classes.
- Added a WireMocha specific line marker icon for the Admin API calls in the Java DSL.
- Extended the max cache entries inlay hint to `WireMockConfiguration.withMaxTemplateCacheEntries()` introduced in WireMock 3.1.0.

### Fixed
- Added missing Java escaping at a couple of places in the Java stub-mapping preview.

## 1.0.15

### Added
- Added an inspection to report `request.url` and `request.urlPath` JSON properties containing regex symbols.
  Those values should potentially be used inside `urlPattern`, `urlPathPattern` or `urlPathTemplate`.
- Added an inspection to report `WireMock.urlEqualTo()` and `WireMock.urlPathEqualTo()` calls containing regex symbols.
  Those values should potentially be used inside `urlMatching()`, `urlPathMatching()` or `urlPathTemplate()`.
- **Testcontainers Java**: Added support for version alpha-7.

### Changed
- Updated some inspection messages to make them more specific.
- Removed some unnecessary logic regarding JSON stub mapping Java preview.

### Fixed
- Fixed an issue with storing and handling different WireMock library versions.

## 1.0.14

### Added
- [60 - **Testcontainers Java**](https://github.com/picimako/wiremocha/issues/60): Added an inspection to detect multiple `WireMockContainer.withName()` and `withFile()/withFileFromResource()`
  calls with the same mapping name or mapping file.
- [60 - **Testcontainers Java**](https://github.com/picimako/wiremocha/issues/60): Added JSON language injection to the `json` parameter of `WireMockContainer.withMapping(name, json)`.
- [60 - **Testcontainers Java**](https://github.com/picimako/wiremocha/issues/60): Added code completion for CLI arguments in `WireMockContainer.withCliArg()`. It displays only those CLI args that are supported
  by the WireMock version specified during instantiation of the `WireMockContainer`.
- [60 - **Testcontainers Java**](https://github.com/picimako/wiremocha/issues/60): Added code completion and reference resolution for WireMock extension classes in `WireMockContainer.withExtension(id, className)`.
- [66 - **WireMock 3.0.0-beta-12**](https://github.com/picimako/wiremocha/issues/66): Call chains starting with a call to `WireMock.requestedFor(String, UrlPattern)` are now supported
  for analyzing duplicate header, body, etc. configuration.
- [66 - **WireMock 3.0.0-beta-12**](https://github.com/picimako/wiremocha/issues/66): Added an inspection to detect `RequestPatternBuilder.withHeader()/withQueryParam()`
  called with `WireMock.absent()` or `MultiValuePattern.absent()`. A quick fix can replace them with a call to `RequestPatternBuilder.withoutHeader()/withoutQueryParam()`.
- Added `ANY` to the selectable request methods in the WireMock tool window for configuring Scenario generation.
- Added the `epoch` and `unix` code completion options to the `date` Handlebars helper's `format` hash.

### Changed
- Improved code completion of CA keystore types and content type headers, so that only the relevant items are presented in `WireMockConfiguration.caKeystoreType()`,
  in `withHeader()` calls, and in header property values in stub mapping files.
- Improved code completion for WireMock extensions in `WireMockConfiguration.extensions()`. Now it only displays actual extension implementation classes.
- Various optimizations under the hood.

## 1.0.13

### Changed
- Mapping file node decoration can now display urls specified via the `request.urlPathTemplate` attribute.
- [64](https://github.com/picimako/wiremocha/issues/64): Updated base64 encoding/decoding logic based on WireMock 3.0.0-beta-11 switching from `com.google.common.io.BaseEncoding` to `java.util.Base64`.
- [64](https://github.com/picimako/wiremocha/issues/64): Added the `serveEventListeners` property to the stub mapping JSON schema.
- [64](https://github.com/picimako/wiremocha/issues/64): Added the `serveEventListeners` property to the set of supported properties for conversion to Java code.
- [64](https://github.com/picimako/wiremocha/issues/64): The `ResponseTemplateTransformer.Builder.maxCacheEntries()` inlay hint is now extended to be valid only until WireMock 2.35.0.
  If you are using WireMock 3.0.0, it is advised to turn the inlay hint off under `Settings` / `Editor` / `Inlay Hints` / `Other` / `Java` / `WireMock: ResponseTemplateTransformer.maxCacheEntries`.
- Added further checks to make sure providing schema for JSON stub mapping files doesn't fail when the conditions are not proper.

### Fixed
- [63](https://github.com/picimako/wiremocha/issues/63): Attempt to fix an exception thrown when updating the Java code preview during a potential document save.
- Fixed a `NullPointerException` during converting WireMock `Parameters` in the Java code preview.
- Fixed the conversion of missing and empty `Parameters` objects in `customMatchers`, in the Java code preview.
- [65](https://github.com/picimako/wiremocha/issues/65): The `response.jsonBody` property now permits array values as well besides objects.
- Added JSON escaping to the value of `response.jsonBody` when converting it to a `withBody()` call in the Java code preview.
- 
## 1.0.12

### Added
- [58](https://github.com/picimako/wiremocha/issues/58): Added the first iteration of a feature that can generate IntelliJ HTTP Client requests
  from WireMock Admin API related calls from Java code.
- [59](https://github.com/picimako/wiremocha/issues/59): Added WireMock 3 beta-9 form parameter configuration conversion (JSON to Java) to the stub mapping preview.
- [59](https://github.com/picimako/wiremocha/issues/59): Added `request.formParameters` property to the stub mapping JSON schema.
- Offer this plugin when a project has dependency on either `com.github.tomakehurst:wiremock` or `com.github.tomakehurst:wiremock-jre8`.

### Changed
- Dropped support for IJ 2021.3.
- Temporarily disabled the JsonPath language injection for the `matchesJsonPath` JSON property, due to certain changes in the IntelliJ Platform.
- Improved the generation of JUnit4 Rule fields and JUnit5 extension fields. Now, they are generated right after the field preceding the caret position.
- Improved the Admin API related live templates with macros.
- Improved the local variable creation in the JSON-Java stub mapping preview for `Parameters`, `Metadata` and `PostServeActionDefinition`s. Now when suitable,
  the `Map.of()` initializer is used instead of `new HashMap<>()`.

### Fixed
- Fixed a `ClassNotFoundException` for an invalid plugin extension configuration.

## 1.0.11

*WireMocha has been supporting IntelliJ since version 2021.3, but to be able to provide better functionality and performance,
v1.0.12 will be the first release in which 2021.3 will no longer be supported.*

*Starting from v1.0.12, with each new major release of IntelliJ, the earliest supported major IDE version will be bumped.*
***In v1.0.11 nothing has changed on this part, this is just a heads-up for the future.***

### Added
- [52](https://github.com/picimako/wiremocha/issues/52) WireMock 3.0.0-beta-8 support:
  - Added default json schema version reporting for `WireMock.matchingJsonSchema()`.
  - Added support for converting `matchesJsonSchema` properties to `WireMock.matchingJsonSchema()` in Java code preview.
- [50](https://github.com/picimako/wiremocha/issues/50): Added `scheme`, `host`, `port`, `customMatcher` and `multipartPatterns` properties
  to the request mapping JSON schema for improved validation and code completion of them in the IDE.

### Changed
- Added preliminary support for IntelliJ 2023.2 EAP.

### Fixed
- Fixed the json stub mapping object up-down mover for cases when the switched mappings are of different length.
- [53](https://github.com/picimako/wiremocha/issues/53): Fixed the code completion for Content-Type header values
  inside `response.headers.Content-Type.*` properties. Both string and string-array header values are supported as locations for completion.

## 1.0.10

### Added
- [27](https://github.com/picimako/wiremocha/issues/27): Added Java code generation and preview during editing JSON stub mapping files.
  This preview is a split panel similar to how for example Markdown file previewing works. It is disabled by default and can be
  enabled in the IDE Settings.

### Changed
- [48](https://github.com/picimako/wiremocha/issues/48): JSON schemas are now provided based on the versions of WireMock used
  in each module of a project.
- Added validation to report if there is at least one empty path variable (i.e. `{}`) in `request.urlPathTemplate` or `WireMock.urlPathTemplate()`.
- Improved and simplified the classification of stub mapping JSON files. Neither single- nor multi-mapping files are considered incomplete in any case.

## 1.0.9

### Added
- Added up-down mover for stub mapping objects in multi-mapping files. Using the Ctrl+Shift+Up/Down shortcuts or similar actions, when the
  caret is placed right before/after the opening curly of a stub-mapping object, the entire object can be moved up/down.
- [40](https://github.com/picimako/wiremocha/issues/40):
  - Added support for converting `withBody()` call arguments to base64 when
    the argument consists of a constant reference, or multiple string literals or constants combined with the + operation.
  - Added support for the response body extraction intention action to support text blocks and combined string literals and constants.
  - If the project JDK is version 17 or above, inlining a response body file will happen into a text block instead of a regular string literal.
- [42](https://github.com/picimako/wiremocha/issues/42): Added code completion for Content-Type header values in `withHeader()` calls of
  `ResponseDefinitionBuilder` and `MappingBuilder`, as well as in JSON stub-mapping files inside `request/response.headers.Content-Type.*` properties.
- [43](https://github.com/picimako/wiremocha/issues/43) - WireMock 3.0.0-beta3 support:
  - Added the `urlPathTemplate` and `pathParameters` properties to the stub mapping JSON schema.
  - Extended the duplicate `request.url*` property analysis with `urlPathTemplate`.
  - Added an intention action that can generate `request.pathParameters` and matchers inside for the path variables in the `request.urlPathTemplate` property.
  - Added an intention action that can generate `withPathParam()` calls with matchers inside for the path variables in the `WireMock.urlPathTemplate()` call.
  - Added an inspection to report the `request.pathParameters` property if there is no `request.urlPathTemplate` property specified in the same stub mapping.
  - Added inspections to report the value of the `request.urlPathTemplate` property and the `WireMock.urlPathTemplate()` call if there is no path variable defined in them.
  - Added inspections to report when `request.pathParameters`/`withPathParam()` calls have one or more param name specified that doesn't exist in `request.urlPathTemplate`/`WireMock.urlPathTemplate()`.
- [45](https://github.com/picimako/wiremocha/issues/43) - WireMock 3.0.0-beta5 support:
  - Added default argument value validation for `WireMockConfiguration.proxyTimeout()` and `WireMockConfiguration.proxyPassThrough()`.
  - Added code completion for the `request.clientIp` Handlebars helper attribute, and fixed the list of displayed code completion options for `request`.

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
