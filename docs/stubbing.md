# Stubbing

<!-- TOC -->
* [Simplify response related stubbing](#simplify-response-related-stubbing)
* [Incorrect configuration in response definitions](#incorrect-configuration-in-response-definitions)
* [Response definition body file paths](#response-definition-body-file-paths)
  * [Gutter icons](#gutter-icons)
  * [References](#references)
* [Convert between various response body definition modes](#convert-between-various-response-body-definition-modes)
  * [Inline the contents of a body file as a body string](#inline-the-contents-of-a-body-file-as-a-body-string)
  * [Extract string body into a body file](#extract-string-body-into-a-body-file)
  * [Base64-encode body string](#base64-encode-body-string)
  * [Decode base64-encoded body](#decode-base64-encoded-body)
* [Scenario generation](#scenario-generation)
<!-- TOC -->

## Simplify response related stubbing

![](https://img.shields.io/badge/inspection-orange) ![](https://img.shields.io/badge/since-1.0.0-blue)

There are many convenience methods available in the `WireMock` class to simplify and shorten stubbing of responses.

This inspection reports cases when response specific stubbing may be replaced with a shorter version, also providing a quick fix for each replacement.

NOTE: Since the form `WireMock.aResponse().withStatus(...)` (for status codes that do not have convenience methods) might be more descriptive than using `WireMock.status(...)`,
there is a flag in the inspection's settings whether to report such cases. It is disabled by default.

**Status 2xx**

```java
From: aResponse().withStatus(200)
  to: ok()

From: aResponse().withStatus(200).withBody(body)
      ok().withBody(body)
  to: ok(body)

From: aResponse().withStatus(200).withHeader(CONTENT_TYPE, contentType).withBody(body)
      ok().withBody(body).withHeader(CONTENT_TYPE, contentType)
      ok(body).withHeader(CONTENT_TYPE, contentType)
  to: okForContentType(contentType, body)

From: aResponse().withStatus(201)
  to: created()

From: aResponse().withStatus(204)
  to: noContent()

From: okForContentType("application/json", body)
  to: okJson(body)

From: okForContentType("application/xml", body)
  to: okXml(body)

From: okForContentType("text/xml", body)
  to: okTextXml(body)
```

**Status 3xx**

```java
From: aResponse().withStatus(301).withHeader("Location", location)
  to: permanentRedirect(location)

From: aResponse().withStatus(302).withHeader("Location", location)
  to: temporaryRedirect(location)

From: aResponse().withStatus(303).withHeader("Location", location)
  to: seeOther(location)
```

**Status 4xx**

```java
From: aResponse().withStatus(400)
  to: badRequest()

From: aResponse().withStatus(401)
  to: unauthorized()

From: aResponse().withStatus(403)
  to: forbidden()

From: aResponse().withStatus(404)
  to: notFound()

From: aResponse().withStatus(422)
  to: badRequestEntity()
```

**Status 5xx**

```java
From: aResponse().withStatus(500)
  to: serverError()

From: aResponse().withStatus(503)
  to: serviceUnavailable()
```

**Other**

```java
From: aResponse().withStatus(status)
  to: status(status)
```

## Incorrect configuration in response definitions

![](https://img.shields.io/badge/inspection-orange) ![](https://img.shields.io/badge/since-1.0.0-blue)

This inspection is an umbrella for reporting any issue that is deemed incorrect when configuring stub response definitions.

Currently, it reports `ResponseDefinitionBuilder#withBodyFile()` calls whose path parameters start with the `/` symbol.
According to the [WireMock Stubbing documentation](http://wiremock.org/docs/stubbing/):
> Body file paths should always be relative i.e. not have a leading /

**Java**

```java
public class ATest {
    private static final String INCORRECT_PATH = "/incorrect/path";

    void method() {
        stubFor(post("/").willReturn(aResponse().withBodyFile("/incorrect/path")));
        stubFor(post("/").willReturn(aResponse().withBodyFile(INCORRECT_PATH)));
    }
}
```

**JSON**

```json
{
  "request": { ... },
  "response": {
    "status": 200,
    "bodyFileName": "/incorrect/path"
  }
}
```

## Response definition body file paths

### Gutter icons

![](https://img.shields.io/badge/linemarker-orange) ![](https://img.shields.io/badge/since-1.0.0-blue)

`ResponseDefinitionBuilder#withBodyFile()` takes argument a path, relative to a `__files` directory (by default `src/test/resources/__files`,
see [Specifying the response body](http://wiremock.org/docs/stubbing/#specifying-the-response-body)), to a file providing the contents of the response body.

In JSON mappings the `response.bodyFileName` property behaves the same.

To mark the location of such files, a line marker/gutter icon is added if the path can be resolved, to which the file reference is always added.

#### Java

The icon is displayed if the path can be resolved to any of the `__files` directories in the project, if there are multiple.

![response_definition_with_body_file_gutter_icon_java](assets/response_definition_with_body_file_gutter_icon_java.png)

#### JSON

The icon is displayed if the path can be resolved to the `_files` directory sibling of the mapping file's parent (at any level) `mappings` directory.

![response_definition_with_body_file_gutter_icon_json](assets/response_definition_with_body_file_gutter_icon_json.PNG)

### References

![](https://img.shields.io/badge/reference-orange) ![](https://img.shields.io/badge/since-1.0.5-blue)

Similarly to the gutter icons detailed in the previous section, references are also provided for the file paths (both in JSON and Java DSLs),
so that user can navigate to them via Ctrl+Click and similar actions.

Such references also make it possible for the response body files to show up in usage based searches, for instance when removing the response body file,
but its path is still references in mapping files, or test code.

## Convert between various response body definition modes

WireMock provides multiple ways to specify response bodies, so naturally there can be various ways of conversion between them.  The aim of the following intentions is to help with those conversions.

NOTE: currently, whether a JSON file is an actual mapping file, is determined by whether it is located (directly or indirectly) in a folder called **mappings**.

### Inline the contents of a body file as a body string

![](https://img.shields.io/badge/intention-orange) ![](https://img.shields.io/badge/since-1.0.0-blue)

The intentions below are available when:
- **Java DSL**: the caret is at the `withBodyFile()` method call identifier, and the file path can be resolved to a valid file in any of the `__files` folders within the project
- **JSON DSL**: at any part of the `response.bodyFileName` property, and the file path can be resolved to a valid file in the `__files` folder under the same root where the current mapping file is located

Upon conversion, the JSON string is not pretty-printed or minified, it will contain line breaks, indentation, etc., but it is escaped according to Java/JSON escaping rules.

Also, there is no check for the usage of the source file it was converted from whether it could be removed or not.

**Java**

It converts `ResponseDefinitionBuilder#withBodyFile()` calls to `withBody()` calls replacing the referenced file path with the contents of the file as the argument body.

If the file path is referenced via a constant, the constant is kept even if it is not used anywhere else. It is up to the users to decide
whether they want to keep it or delete it.

The intention can locate stub files in any of the `__files` folders in the current project. In case multiple of them is found on the same relative path,
users can choose which one they want to inline the contents of.

```java
from: aResponse().withBodyFile("com/wiremocha/a_response_body.json");
to:   aResponse().withBody("{\n  \"name\": \"value\"\n}"); //given that a_response_body.json contained that text
```

![ResponseDefinitionBuilder_withBodyFile_to_withBody](assets/convert_ResponseDefinitionBuilder_withBodyFile_to_withBody.gif)

**JSON**

This one converts `response.bodyFileName` properties to `body` ones replacing the referenced file path with the contents of the file as the property value.

```json
//from:
"response": {
  "bodyFileName": "com/wiremocha/a_response_body.json"
}

//to:
"response": {
  "body": "{\n  \"name\": \"value\"\n}"
}
```

![response_definition_bodyFileName_to_body](assets/convert_response_definition_bodyFileName_to_body.gif)

### Extract string body into a body file

![](https://img.shields.io/badge/intention-orange) ![](https://img.shields.io/badge/since-1.0.0-blue)

This intention makes it possible to extract a response body string into a separate file, and replace it with its relative path to the `__files` directory.

#### Java

It replaces `ResponseDefinitionBuilder#withBody(<response body string>)` calls with `withBodyFile(<relative path to __files directory>)` ones along with creating
the new response body file in the target directory.

It is available when the selected method call is `ResponseDefinitionBuilder#withBody(String)`, and there is at least one `__files` directory in the current project.

**Workflow/extraction process**

![response_body_extraction_process](assets/response_body_extraction_process.png)

```java
from: aResponse().withBody("{\n    \"name\": \"value\"\n}");
to:   aResponse().withBodyFile("com/wiremocha/a_response_body.json");

//The path "com/wiremocha/a_response_body.json" is relative to a __files directory wherever it is located in the project.
```

#### JSON

The logic is pretty much the same for the JSON DSL with the following differences:

- It is available when the selected property is `response.body` with a string literal value, in an actual mapping file, and there is a `__files` directory under the same root as the mapping file.
- The body file is created in the `__files` directory under the same root as the mapping file is located.
(However, since a target sub-folder can be selected also, when there is any, there is no restriction to extract the body to any location.)

```json
//from:
"response": {
  "body": "\"{\n  \"some\": \"json\"\n}\""
}

//to:
"response": {
  "bodyFileName": "com/wiremocha/a_response_body.json"
}
```

### Base64-encode body string

![](https://img.shields.io/badge/intention-orange) ![](https://img.shields.io/badge/since-1.0.0-blue)

These intentions help replace string literal response bodies with their base64-encoded values. The encoding logic is based on the
[`GuavaBase64Encoder`](https://github.com/wiremock/wiremock/blob/master/src/main/java/com/github/tomakehurst/wiremock/common/GuavaBase64Encoder.java)
class what WireMock uses for encoding.

And, since padding can be kept or omitted, there are separate intentions for the two modes.

**Java**

It converts `ResponseDefinitionBuilder#withBody()` calls to `#withBase64Body()` calls replacing the body with its base64-encoded value,
and is available when the caret is at the `withBody()` method call identifier.

```java
from:              aResponse().withBody("{\"aJson\": \"string\"}");
to (with padding): aResponse().withBase64Body("eyJhSnNvbiI6ICJzdHJpbmcifQ==");
to (w/o padding):  aResponse().withBase64Body("eyJhSnNvbiI6ICJzdHJpbmcifQ");
```

**JSON**

It converts `response.body` properties to `response.base64Body` ones in JSON mapping files, replacing the body with its base64-encoded value.
It is available when the caret is at the `response.body` property, and the argument value is a string literal.

```json
//from:
"response": {
  "body": "{\"aJson\": \"string\"}"
}

//to (with padding):
  "base64Body": "eyJhSnNvbiI6ICJzdHJpbmcifQ=="

//to (w/o padding):
  "base64Body": "eyJhSnNvbiI6ICJzdHJpbmcifQ"
```

### Decode base64-encoded body

![](https://img.shields.io/badge/intention-orange) ![](https://img.shields.io/badge/since-1.0.0-blue)

These intentions help replace base64-encoded response bodies with their decoded values. The decoding logic is based on the
[`GuavaBase64Encoder`](https://github.com/wiremock/wiremock/blob/master/src/main/java/com/github/tomakehurst/wiremock/common/GuavaBase64Encoder.java)
class what WireMock uses for decoding.

If, upon invoking the intention, the argument cannot be decoded due to being an invalid base64 string, an error hint is displayed with the reason.

**Java**

It converts `ResponseDefinitionBuilder#withBase64Body()` calls to `#withBody()` calls replacing the base64-encoded string with its decoded value,
and is available when the caret is at the `withBase64Body()` method call identifier.

```java
from: aResponse().withBase64Body("eyJhSnNvbiI6ICJzdHJpbmcifQ==");
to:   aResponse().withBody("{\"aJson\": \"string\"}");
```

**JSON**

It converts `response.base64Body` properties to `response.body` ones in JSON mapping files, replacing the base64-encoded string with its decoded value.
It is available when the caret is at the `response.base64Body` property, and the argument value is a string literal.

```json
//from:
"response": {
  "base64Body": "eyJhSnNvbiI6ICJzdHJpbmcifQ=="
}

//to:
"response": {
  "body": "{\"aJson\": \"string\"}"
}
```

## Scenario generation

![](https://img.shields.io/badge/toolwindow-orange) ![](https://img.shields.io/badge/since-1.0.4-blue)

The WireMock tool window can be found at the bottom of the IDE frame, and consists of one tab, called **Generate Scenario**.

The aim of this tool is to generate *skeleton* Java and JSON stub mappings that are tied to a specific scenario.
It may help users to concentrate on modeling the scenario states first, without having to deal with the coding aspect in that state
of the implementation.

### Scenario definition

On the left side of the tool, you can specify a scenario name that is being modeled, and stub definitions with the following attributes/columns 
**Method**, **URL**, **Current State** and **New State**. Response code is not customizable at the moment.

On the right side, the code is automatically generated and updated upon modifications on the left side.

Each row of the table reads like this: Sending a **Method**-type request to **URL** in **Current State** will return some response, and optionally move the scenario to **New State**.

There is no validation on any of the fields, but for convenience, if the **Current State** is a case-insensitive variant of `Scenario.STARTED` or `STARTED`,
then the following values are generated into the code:
- Java: `Scenario.STARTED`
- JSON: `Started`

Note: the Method dropdowns are editable when double-clicked. 

### Toolbar actions

In addition to the standard Add, Remove and Move Up/Down actions, the following extra action are available:
- **Add Stub Definition with Started State**: adds a new table row whose Current State is set to `STARTED`. 
- **Transition from Previous New State**: adds a new table row whose Current State is the New State of the selected row.
- **Duplicate Row(s)**: duplicates the selected rows. The new rows are added to the end of the table.

**Example** (based on the [WireMock Stateful Behaviour](https://wiremock.org/docs/stateful-behaviour/) documentation):

![generate_scenario_tool_window](assets/generate_scenario_based_stub_mappings.gif)
