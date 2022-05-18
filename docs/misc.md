# Miscellaneous

## Default method argument values

![](https://img.shields.io/badge/inspection-orange) ![](https://img.shields.io/badge/since-1.0.2-blue)

This inspection reports method argument values that match the default values of properties those methods set.

Methods in the following classes are analyzed:
- `com.github.tomakehurst.wiremock.recording.RecordSpecBuilder`
- `com.github.tomakehurst.wiremock.client.WireMockBuilder`
- `com.github.tomakehurst.wiremock.core.WireMockConfiguration`
- `com.github.tomakehurst.wiremock.matching.MultipartValuePatternBuilder`
- `com.github.tomakehurst.wiremock.junit5.WireMockExtension.Builder`

**Example:**

```java
WireMockConfiguration.wireMockConfig()
    .timeout(300000) //reported due to default value
    .port(9090) //not reported
    .httpDisabled(false); //reported due to default value
```
