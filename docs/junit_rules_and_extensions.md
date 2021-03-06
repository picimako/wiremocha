# JUnit Rules and Extensions

- [JUnit 4 and JUnit 5 Vintage](#junit-4-and-junit-5-vintage)
  - [Generate Rule and ClassRule fields](#generate-rule-and-classrule-fields)
    - [WireMock JUnit 4 Rule](#wiremock-junit-4-rule)
    - [WireMock JUnit 4.10 Class Rule](#wiremock-junit-410-class-rule)
    - [WireMock JUnit 4.11 Class Rule](#wiremock-junit-411-class-rule)
- [JUnit 5](#junit-5)
  - [Generate WireMockExtension field](#generate-wiremockextension-field)
  - [Convert test classes between the declarative and programmatic approaches](#convert-test-classes-between-the-declarative-and-programmatic-approaches)
    - [From declarative to programmatic](#from-declarative-to-programmatic)
    - [From programmatic to declarative](#from-programmatic-to-declarative)
  - [@WireMockTest annotation](#wiremocktest-annotation)
    - [Inlay hints](#inlay-hints)
    - [Inspections](#inspections)

WireMock provides various JUnit Rule and Extension implementations for [JUnit 4, JUnit 5 Vintage](http://wiremock.org/docs/junit-extensions/) and [JUnit 5 Jupiter](http://wiremock.org/docs/junit-jupiter/) as well.
These are manifested as `@Rule`, `@ClassRule` or `@RegisterExtension` annotated class and instance fields in the test classes.

The plugin provides dedicated actions in the <kbd>Alt</kbd>+<kbd>Insert</kbd> **Generate** menu to insert such fields according to the
JUnit framework versions suitable for the current test class.

These code insertions are based on dedicated Code Templates, and are applied in the form of on-the-fly created Live Templates.

## JUnit 4 and JUnit 5 Vintage

### Generate Rule and ClassRule fields

![](https://img.shields.io/badge/action-orange) ![](https://img.shields.io/badge/since-1.0.0-blue)

In case of class rules, the [official WireMock documentation](http://wiremock.org/docs/junit-extensions/) and WireMock itself differentiates and has different
logic for JUnit 4.10 and earlier versions, and for versions later than 4.10 due to some constraints in later versions: according to the official documentation
> *"Unfortunately JUnit 4.11 and above prohibits @Rule on static members so a slightly more verbose form is required."*

Since [JUnit 5 Vintage requires JUnit 4.12 or later version to be on the classpath](https://junit.org/junit5/docs/current/user-guide/#overview-what-is-junit-5) it is implicitly
supported by the JUnit 4 specific actions.

Generally, these actions are available when only JUnit 4 is suitable for the current test class. More specific criteria are described in the table below.

All WireMock code templates are available under <kbd>IDE Settings</kbd> > <kbd>Editor</kbd> > <kbd>File and Code Templates</kbd> > <kbd>Code</kbd>.

#### WireMock JUnit 4 Rule

Available regardless of the library version.

```java
@org.junit.Rule
public com.github.tomakehurst.wiremock.junit.WireMockRule ${NAME} = new WireMockRule()
```

Template variables:
- `${NAME}`: Name of the created field.

#### WireMock JUnit 4.10 Class Rule

Available when JUnit 4.10 or earlier (Not necessarily exclusively, other versions of 4.x might also be present.) is on the classpath.

```java
@org.junit.ClassRule
@org.junit.Rule
public static com.github.tomakehurst.wiremock.junit.WireMockClassRule ${NAME} = new WireMockClassRule();
```

Template variables:
- `${NAME}`: Name of the created field.

#### WireMock JUnit 4.11 Class Rule

Available when JUnit 4.11 or later (Not necessarily exclusively, other versions of 4.x might also be present.) is on the classpath.

```java
@org.junit.ClassRule
public static com.github.tomakehurst.wiremock.junit.WireMockClassRule ${CLASS_RULE_NAME} = new WireMockClassRule();
@org.junit.Rule
public WireMockClassRule ${RULE_NAME} = ${CLASS_RULE_NAME};
```

Template variables:
- `${CLASS_RULE_NAME}`: Name of the class rule field.
- `${RULE_NAME}`: Name of the instance rule field.

NOTES: 
- Upon insertion, all template variables suggest potentially suitable field names.

## JUnit 5

### Generate WireMockExtension field

![](https://img.shields.io/badge/action-orange) ![](https://img.shields.io/badge/since-1.0.0-blue)

JUnit 5 is different in that it doesn't have Rules anymore, instead it works with so-called extensions, for which WireMock has
the dedicated `com.github.tomakehurst.wiremock.junit5.WireMockExtension` type that can be added as a field,
annotated with `@org.junit.jupiter.api.extension.RegisterExtension`, in test classes. See [WireMock JUnit 5 documentation](http://wiremock.org/docs/junit-jupiter/).

The action is available when only JUnit 5, regardless of the library version, is suitable for the current test class.

```java
@org.junit.jupiter.api.extension.RegisterExtension
static com.github.tomakehurst.wiremock.junit5.WireMockExtension ${NAME} = WireMockExtension.newInstance().options(${OPTIONS}).build();
```

Template variables:
- `${NAME}`: Name of the created field. Upon insertion, it suggests potentially suitable field names.
- `${OPTIONS}`: The options configuration with a default value of `com.github.tomakehurst.wiremock.core.WireMockConfiguration.wireMockConfig()`.

### Convert test classes between the declarative and programmatic approaches

For JUnit 5 WireMock has [a declarative and a programmatic mode](http://wiremock.org/docs/junit-jupiter/) for running one or more WireMock instances.

#### From declarative to programmatic

![](https://img.shields.io/badge/intention-orange) ![](https://img.shields.io/badge/since-1.0.0-blue)

This intention can convert the declarative variant to the programmatic one.
It is available on `@WireMockTest` annotated Java classes when there is no `WireMockExtension` field defined in the class.

The intention performs the following changes:
- creates a `WireMockExtension` field configured based on the attribute values in the `@WireMockTest` annotation,
- replaces each occurrence of `WireMock.stubFor()` with `wm.stubFor()` (`wm` is the name of the newly created field), so that the new field is referenced in the test code,
- removes the `@WireMockTest` annotation from the class.

The configuration of the field happens as follows:

| Case                     | From                                                   | To                                                                                                                                                                                                                        |
|--------------------------|--------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Default configuration    | `@WireMocktest`                                        | <pre>@RegisterExtension<br/>static WireMockExtension wm = WireMockExtension.newInstance().build();</pre>                                                                                                                  |
| httpPort                 | `@WireMocktest(httpPort = 1234)`                       | <pre>@RegisterExtension<br/>static WireMockExtension wm = WireMockExtension.newInstance()<br/>  .options(WireMockConfiguration.wireMockConfig().port(1234))<br/>  .build();</pre>                                         |
| httpsEnabled             | `@WireMocktest(httpsEnabled = true)`                   | <pre>@RegisterExtension<br/>static WireMockExtension wm = WireMockExtension.newInstance()<br/>  .options(WireMockConfiguration.wireMockConfig()<br/>    .dynamicPort()<br/>    .dynamicHttpsPort())<br/>  .build();</pre> |
| httpsPort                | `@WireMocktest(httpsPort = 1234)`                      | Since `httpsEnabled` is false, `httpsPort` is not set at all:<br/><pre>@RegisterExtension<br/>static WireMockExtension wm = WireMockExtension.newInstance().build();</pre>                                                |
| httpsEnabled + httpsPort | `@WireMocktest(httpsEnabled = true, httpsPort = 1234)` | <pre>@RegisterExtension<br/>static WireMockExtension wm = WireMockExtension.newInstance()<br/>  .options(WireMockConfiguration.wireMockConfig()<br/>    .dynamicPort()<br/>    .httpsPort(1234))<br/>  .build();</pre>    |
| proxyMode                | `@WireMocktest(proxyMode = true)`                      | <pre>@RegisterExtension<br/>static WireMockExtension wm = WireMockExtension.newInstance()<br/>  .proxyMode(true)<br/>  .build();</pre>                                                                                    |

#### From programmatic to declarative

![](https://img.shields.io/badge/intention-orange) ![](https://img.shields.io/badge/since-1.0.0-blue)

This intention can convert the programmatic variant to the declarative one.

The intention is available
- on Java classes **not** annotated with `@WireMockTest`,
- when there is only one `WireMockExtension` field defined in the class,
- and when there is no call on `WireMockExtension.Builder` or `WireMockConfiguration` that is not supported by the `@WireMockTest` annotation.

The intention performs the following changes:
- Creates a `@WireMockTest` annotation configured based on the `WireMockExtension` field's initializer, and adds that annotation
to the target class.
- Replaces each occurrence of `wm.stubFor()` (`wm` is the name of the source field) with `WireMock.stubFor()`, so that the field references are removed.
- Removes the source `WireMockExtension` field from the class.

The configuration of the annotation happens as follows:

| Case                  | From                                                                                                                                                                                | To                                                     |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------|
| Default configuration | <pre>@RegisterExtension<br/>static WireMockExtension wm = WireMockExtension.newInstance().build();</pre>                                                                            | `@WireMockTest`                                        |
| httpPort              | <pre>@RegisterExtension<br/>static WireMockExtension wm = WireMockExtension.newInstance()<br/>  .options(wireMockConfig().port(8090))<br/>  .build();</pre>                         | `WireMockTest(httpPort = 8090)`                        |
|                       | <pre>@RegisterExtension<br/>static WireMockExtension wm = WireMockExtension.newInstance()<br/>  .options(wireMockConfig().dynamicPort().port(8090))<br/>  .build();</pre>           | `WireMockTest(httpPort = 8090)`                        |
| Dynamic httpPort      | <pre>@RegisterExtension<br/>static WireMockExtension wm = WireMockExtension.newInstance()<br/>  .options(wireMockConfig().dynamicPort())<br/>  .build();</pre>                      | `@WireMockTest`                                        |
| proxyMode enabled     | <pre>@RegisterExtension<br/>static WireMockExtension wm = WireMockExtension.newInstance()<br/>  .proxyMode(true)<br/>  .build();</pre>                                              | `@WireMockTest(proxyMode = true)`                      |
| proxyMode disabled    | <pre>@RegisterExtension<br/>static WireMockExtension wm = WireMockExtension.newInstance()<br/>  .proxyMode(false)<br/>  .build();</pre>                                             | `@WireMockTest`                                        |
| Dynamic httpsPort     | <pre>@RegisterExtension<br/>static WireMockExtension wm = WireMockExtension.newInstance()<br/>  .options(wireMockConfig().dynamicHttpsPort())<br/>  .build();</pre>                 | `@WireMockTest(httpsEnabled = true)`                   |
| httpsPort             | <pre>@RegisterExtension<br/>static WireMockExtension wm = WireMockExtension.newInstance()<br/>  .options(wireMockConfig().dynamicHttpsPort().httpsPort(8090))<br/>  .build();</pre> | `@WireMockTest(httpsEnabled = true, httpsPort = 8090)` |
|                       | <pre>@RegisterExtension<br/>static WireMockExtension wm = WireMockExtension.newInstance()<br/>  .options(wireMockConfig().httpsPort(8090))<br/>  .build();</pre>                    | `@WireMockTest(httpsEnabled = true, httpsPort = 8090)` |

### @WireMockTest annotation

#### Inlay hints

![](https://img.shields.io/badge/inlayhint-orange) ![](https://img.shields.io/badge/since-1.0.0-blue)

Based on WireMock's [JUnit5 usage documentation](http://wiremock.org/docs/junit-jupiter/), when either the `httpPort` or the `httpsPort` attribute is not specified, random ports are used.

To inform users about this, an inlay hint is displayed on top of the `@WireMockTest` annotation if either one of those attributes is not specified:

| httpPort specified | httpsEnabled | httpsPort specified | Inlay hint text              |
|--------------------|--------------|---------------------|------------------------------|
| false              | false/true   | false/true          | dynamic HTTP port            |
| false              | true         | false               | dynamic HTTP and HTTPS ports |
| true               | true         | false               | dynamic HTTPS port           |

In any other case no hint is displayed.

#### Inspections

![](https://img.shields.io/badge/inspection-orange) ![](https://img.shields.io/badge/since-1.0.0-blue)

This inspection reports issues regarding the configuration of the `@WireMockTest` annotation.

Currently, it reports the `httpsPort` attribute if the `httpsEnabled` attribute is set to false either explicitly or by default, because the HTTPS port number takes effect
only if HTTPS is enabled.
Since v1.0.1 a quick fix is also available to enable HTTPS (set the `httpsEnabled` attribute to true).

```java 
From: @WireMockTest(httpsEnabled = false, httpsPort = 2345)
  to: @WireMockTest(httpsEnabled = true, httpsPort = 2345)

From: @WireMockTest(httpsPort = 2345)
  to: @WireMockTest(httpsPort = 2345, httpsEnabled = true)
```
