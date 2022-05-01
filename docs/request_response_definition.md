# Request and response definitions

- [Duplicate configuration in response definitions (Java DSL)](#duplicate-configuration-in-request-and-response-definitions-java-dsl)
- [Duplicate configuration in response definitions (JSON DSL)](#duplicate-configuration-in-response-definitions-json-dsl)

## Duplicate configuration in request and response definitions (Java DSL)

![](https://img.shields.io/badge/inspection-orange)

This inspection reports duplicate configuration in `ResponseDefinitionBuilder` call chains.

### withHeader()

![](https://img.shields.io/badge/ResponseDefinitionBuilder-1.0.0-blue)
![](https://img.shields.io/badge/MappingBuilder-1.0.2-blue)
![](https://img.shields.io/badge/RequestPatternBuilder-1.0.2-blue)

When a header name is specified in multiple `.withHeader()` calls, that header's value would be overridden.

#### Via ResponseDefinitionBuilder

```java
//Both "Accept-Language" string literals are highlighted
aResponse().withHeader("Accept-Language", "HU").withHeader("Content-Length", "1024").withHeader("Accept-Language", "JP");

//Both ACCEPT_LANGUAGE constant expressions are highlighted
aResponse().withHeader(ACCEPT_LANGUAGE, "HU").withHeader("Content-Length", "1024").withHeader(ACCEPT_LANGUAGE, "JP");
```

A quick fix is also available (since v1.0.1) to remove duplicate calls for the header name, keeping the call that the user invokes the quick fix on:

```java
//From (where 'Accept-Language: HU' is selected to be kept):
aResponse().withHeader("Accept-Language", "HU").withHeader("Accept-Language", "JA");
//to:
aResponse().withHeader("Accept-Language", "HU");

//From (where 'Accept-Language: JA' is selected to be kept):
aResponse()
    .withHeader("Accept-Language", "HU")
    .withHeader("Content-Length", "1024")
    .withBodyFile("filename")
    .withHeader("Accept-Language", "JA")
    .withHeader("Accept-Language", "RU");
//to:
aResponse()
    .withHeader("Content-Length", "1024")
    .withBodyFile("filename")
    .withHeader("Accept-Language", "JA");
```

#### Via MappingBuilder

The logic is the same here, including the quick fix.

```java
//From (where 'Accept-Language: HU' is selected to be kept):
stubFor(get(urlEqualTo("/path"))
    .withHeader("Accept-Language", equalTo("HU"))
    .withHeader("Accept-Language", equalTo("JA"))
    .willReturn(aResponse()));
//to:
stubFor(get(urlEqualTo("/path"))
    .withHeader("Accept-Language", equalTo("HU"))
    .willReturn(aResponse()));
```

#### Via RequestPatternBuilder

The logic is the same here too, including the quick fix.

```java
//From (where 'Accept-Language: HU' is selected to be kept):
verify(getRequestedFor(urlEqualTo("/")).withHeader("Accept-Language", equalTo("HU")).withPort(8080).withHeader("Accept-Language", equalTo("JA")));
//to:
verify(getRequestedFor(urlEqualTo("/")).withHeader("Accept-Language", equalTo("HU")).withPort(8080));
```

### withQueryParam

![](https://img.shields.io/badge/since-1.0.2-blue)

When a header name is specified in multiple `.withQueryParam()` calls, that header's value would be overridden.

A quick fix is also available to remove duplicate calls for the query param name, keeping the call that the user invokes the quick fix on.

#### Via MappingBuilder

```java
//From (where 'language=HU' is selected to be kept):
stubFor(get(urlEqualTo("/path"))
    .withQueryParam("language", equalTo("HU"))
    .withQueryParam("language", equalTo("JA"))
    .willReturn(aResponse()));
//to:
stubFor(get(urlEqualTo("/path"))
    .withQueryParam("language", equalTo("HU"))
    .willReturn(aResponse()));
```

#### Via RequestPatternBuilder

```java
//From (where 'language=HU' is selected to be kept):
verify(getRequestedFor(urlEqualTo("/")).withQueryParam("language", equalTo("HU")).withPort(8080).withQueryParam("language", equalTo("JA")));
//to:
verify(getRequestedFor(urlEqualTo("/")).withQueryParam("language", equalTo("HU")).withPort(8080));
```

### ResponseDefinitionBuilder#with\*Body*()

![](https://img.shields.io/badge/since-1.0.1-blue)

When the response body is defined multiple ways (or even multiple times the same way), it can be confusing which body is actually applied, and these definitions will override each other.

This inspection supports the following response body definition calls: `withJsonBody()`, `withBody()`, `withBase64Body()`, `withBodyFile()`.
Regardless of if multiple of the same method, or different methods are called, they are all reported and highlighted if duplicates are found.

```java
stubFor(get("/").willReturn(aResponse().withBodyFile("path/to/a_file.json").withBody("{\"aJson\": \"string\"}")));
```

The related quick fix removes duplicate calls, keeping the call that the user invokes the quick fix on:

```java
//From (where the first 'withBodyFile()' call is selected to be kept):
aResponse().withBodyFile("path/to/a_file.json").withBodyFile("path/to/another_file.json");
//to:
aResponse().withBodyFile("path/to/a_file.json");

//From (where the 'withBody()' call is selected to be kept):
aResponse()
    .withBodyFile("path/to/a_file.json")
    .withBase64Body("eyJhSnNvbiI6ICJzdHJpbmcifQ==")
    .withHeader("Accept-Language", "HU")
    .withBody("{}")
    .withFixedDelay(200)
    .withJsonBody(jsonBody);
//to:
aResponse()
    .withHeader("Accept-Language", "HU")
    .withBody("{}")
    .withFixedDelay(200);
```

## Duplicate configuration in response definitions (JSON DSL)

![](https://img.shields.io/badge/inspection-orange)

This inspection reports duplicate configuration in `response` properties in mapping files.

### \*Body*

![](https://img.shields.io/badge/since-1.0.1-blue)

This inspection is the same as the Java DSL for reporting duplicate response body definitions.

It supports the following response body definition properties: `body`, `base64Body`, `bodyFileName`, `jsonBody`
Regardless of if multiple of the same property, or different properties are specified, they are all reported and highlighted if duplicates are found.

```json
{
  "request": { ... },
  "response": {
    "status": 200,
    "bodyFileName": "a/response_body.json",
    "body": "{\"aJson\": \"string\"}"
  }
}
```

The related quick fix removes duplicate properties, keeping the property that the user invokes the quick fix on:

```json
//From (where 'body' is selected to be kept):
{
  "request": { ... },
  "response": {
    "status": 200,
    "bodyFileName": "a/response_body.json",
    "body": "{\"aJson\": \"string\"}",
    "base64Body": "eyJhSnNvbiI6ICJzdHJpbmcifQ=="
  }
}

//to:
{
  "request": { ... },
  "response": {
    "status": 200,
    "body": "{\"aJson\": \"string\"}"
  }
}
```