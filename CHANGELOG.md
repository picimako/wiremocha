# WireMocha Changelog

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
