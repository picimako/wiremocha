# Request Matching

## Before-after matcher folding

![](https://img.shields.io/badge/codefolding-orange) ![](https://img.shields.io/badge/since-0.1.0-blue)

WireMock provides quite a handful of methods for request matching, among which there are some date-time related ones. The following ones are in scope of this code folding:

- `before(...)`
- `beforeNow()`
- `after(...)`
- `afterNow()`

Chained together with `and()` these code snippets can be quite long, thus this code folding aims to lessen the cognitive load on having to read through the
whole matcher expression.

In case they use inside a `matchingJsonPath`, `matchingXPath` or `matchesXPathWithSubMatcher` matcher, the path is included in the folded placeholder text,
otherwise the subject is marked with the `it` word.

For now, the placeholder creation always takes the text of the after and before expressions, be it string literal, constant, or method call, and not the value
they potentially evaluate to.

The following foldings are in place:

**Inside matchingJsonPath():**
```java
source:  stubFor(post("/").withRequestBody(matchingJsonPath("$.date", before("BEFORE").and(after("AFTER")))));
folding: stubFor(post("/").withRequestBody(matching: "AFTER" < "$.date" < "BEFORE"));

source:  stubFor(post("/").withRequestBody(matchingJsonPath("$.date", before("BEFORE").and(afterNow()))));
folding: stubFor(post("/").withRequestBody(matching: [now] < "$.date" < "BEFORE"));

source:  stubFor(post("/").withRequestBody(matchingJsonPath("$.date", beforeNow().and(after("AFTER")))));
folding: stubFor(post("/").withRequestBody(matching: "AFTER" < "$.date" < [now]));

source:  stubFor(post("/").withRequestBody(matchingJsonPath("$.date", WireMock.and(before("2022-02-02T00:00:00"), after("2020-01-01T00:00:00")))));
folding: stubFor(post("/").withRequestBody(matching: "2020-01-01T00:00:00" < "$.date" < "2022-02-02T00:00:00"));
```

**Referencing constants:**
```java
private static final String AFTER_DATE="AFTER_DATE_CONST";
private static final String BEFORE_DATE="BEFORE_DATE_CONST";

source:  stubFor(post("/").withRequestBody(matchingJsonPath("$.date", before(BEFORE_DATE).and(after(AFTER_DATE)))));
folding: stubFor(post("/").withRequestBody(matching: AFTER_DATE < "$.date" < BEFORE_DATE));
```

**Standalone calls**
```java
source:  LogicalAnd and = WireMock.and(before("BEFORE"), afterNow());
folding: LogicalAnd and = [now] < it < "BEFORE";

source:  LogicalAnd and = before("BEFORE").and(after("AFTER));
folding: LogicalAnd and = "AFTER" < it < "BEFORE";

source:  LogicalAnd and = after("AFTER").and(before("BEFORE"));
folding: LogicalAnd and = "AFTER" < it < "BEFORE";
```

Individual `before(...)` and `after(...)` calls are not folded because the save on reading length is not that significant.

## Nonsense matcher combinations

![](https://img.shields.io/badge/inspection-orange) ![](https://img.shields.io/badge/since-0.1.0-blue)

There are certain matcher combinations that either don't make sense or contradict each other, so this inspection reports those cases.

For now, date-time specific matchers linked together with `and()` are supported. For example:

```java
stubFor(post("/").withRequestBody(matchingJsonPath("$.date", isNow().and(beforeNow()))));
```

The full list of combinations that are reported (the operands are applicable in the reverse order as well):

| Left operand        | Right operand                                                                      |
|---------------------|------------------------------------------------------------------------------------|
| `isNow()`           | `after()`, `before()`, `equalToDateTime()`, `afterNow()`, `beforeNow()`, `isNow()` |
| `equalToDateTime()` | `after()`, `before()`, `equalToDateTime()`, `afterNow()`, `beforeNow()`, `isNow()` |
| `beforeNow()`       | `equalToDateTime()`, `afterNow()`, `beforeNow()`, `isNow()`                        |
| `afterNow()`        | `equalToDateTime()`, `afterNow()`, `beforeNow()`, `isNow()`                        |
| `before()`          | `before()`, `isNow()`, `equalToDateTime()`                                         |
| `after()`           | `after()`, `isNow()`, `equalToDateTime()`                                          |

## Method calls with too few arguments

![](https://img.shields.io/badge/inspection-orange) ![](https://img.shields.io/badge/since-0.1.0-blue)

There are methods that require a minimum number of arguments to be passed. This inspection reports calls to them with a fewer number of arguments required.

| Method           | Required minimum no. of arguments |
|------------------|-----------------------------------|
| `WireMock.and()` | 2                                 |
| `WireMock.or()`  | 2                                 |

## Converting boolean and() and or() between WireMock and StringValuePattern variants

![](https://img.shields.io/badge/intention-orange) ![](https://img.shields.io/badge/since-0.1.0-blue)

`StringValuePattern` type matchers can be combined into complex boolean matchers using `and()` and `or()`,
and since there are static methods `WireMock.and()/or()` and instance methods `StringValuePattern.and()/or()`, it is possible to switch between them,
essentially converting between the two forms.

From the fluent `x.and(y)`/`x.or(y)` forms the conversion is possible only when all operands are combined with `and()` or `or()`, respectively.

The intention is generally available if there are at least two operands specified. Bold text below means a selected code snippet.

**From WireMock-based**

| From                        | To                                    | Availability                                                      |
|-----------------------------|---------------------------------------|-------------------------------------------------------------------|
| `WireMock.and(x, y, z)`     | `x.and(y).and(z)`                     | When cursor is on and().                                          |
| `WireMock.or(x, y, z)`      | `x.or(y).or(z)`                       | When cursor is on or().                                           |

**From fluent**

| From                | To                          | Availability                                                      |
|---------------------|-----------------------------|-------------------------------------------------------------------|
| `x.and(y).and(z)`   | `WireMock.and(x, y, z)`     | When at least one `x.and(y)` portion of a call chain is selected. |
| **x.and(y)**.and(z) | `WireMock.and(x, y).and(z)` | When at least one `x.and(y)` portion of a call chain is selected. |
| `x.or(y).or(z)`     | `WireMock.or(x, y, z)`      | When at least one `x.or(y)` portion of a call chain is selected.  |
| **x.or(y)**.or(z)   | `WireMock.or(x, y).or(z)`   | When at least one `x.or(y)` portion of a call chain is selected.  |

Note: if you want to flip the operands of a fluent call, there is already an intention called *Flip commutative method call* that can do that.

**Mixed**

| From                        | To                                    | Availability             |
|-----------------------------|---------------------------------------|--------------------------|
| `x.and(WireMock.and(y, z))` | `WireMock.and(x, WireMock.and(x, z))` | When cursor is on and(). |
| `WireMock.and(x, y).and(z)` | -                                     | Not available.           |

## Replacing date-time "now" calls with convenience method calls

![](https://img.shields.io/badge/inspection-orange) ![](https://img.shields.io/badge/since-0.1.0-blue)

Since date-time matchers (`after`, `before`, `equalToDateTime`) can accept WireMock's **now offset expression** format, the aforementioned methods
can be called with the String value `"now"` for which there are non-parametrized convenience methods.

| From                     | To                     |
|--------------------------|------------------------|
| `after("now")`           | `WireMock.afterNow()`  |
| `before("now")`          | `WireMock.beforeNow()` |
| `equalToDateTime("now")` | `WireMock.isNow()`     |

Method calls chained to the original ones (e.g. `after("now").and(before("SOME DATE"))`) are kept after applying the quick fix, e.g. `WireMock.afterNow().and(before("SOME DATE"))`.
