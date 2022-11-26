# Mapping files

<!-- TOC -->
* [JSON schema](#json-schema)
* [Request-response information on Project View file nodes](#request-response-information-on-project-view-file-nodes)
* [Create mapping file from template](#create-mapping-file-from-template)
<!-- TOC -->

## JSON schema

![](https://img.shields.io/badge/schema-orange) ![](https://img.shields.io/badge/since-1.0.3-blue)

A JSON schema is bound to JSON mapping files, which automatically provides validation and code completion.
It works both for single- and multi-mapping files.

The schema is based on the original WireMock schemas in
the [WireMock GitHub repository](https://github.com/wiremock/wiremock/tree/master/src/main/resources/swagger/schemas).

NOTE: the validation and auto-completion features are not implemented in this plugin, but in the IntelliJ platform
itself.
It simply provides the configuration for the IntelliJ platform, so that it knows what conditions must a file meet to
assign a certain schema file to it.

## Request-response information on Project View file nodes

![](https://img.shields.io/badge/nodedecoration-orange) ![](https://img.shields.io/badge/since-1.0.5-blue)

To provide a basic overview of what data stub mapping files hold, this node decorator adds some basic information to
stub mapping file nodes in the Project View.

![mapping_file_node_decoration](assets/mapping_file_node_decoration.png)

It provides information on the following parts:
- **request method**: the value of the `request.method` property
- **request url**: the value of, any one of, the following properties first found: `request.url`, `request.urlPath`, `request.urlPattern`, `request.urlPathPattern` 
- **response status**: the value of the `response.status` property

### Classification

Mapping files are classified as follows:
- no mapping
- single mapping
- single mapping incomplete
- multiple mappings
- multiple mappings incomplete

The way and cases which category and what information is displayed also aims to uncover potentially erroneous mapping files and missing properties. 

### No mapping

A mapping file is considered empty, and the file node is decorated with the text *no mapping*, when:

- the file is completely empty,
- the root-level value is not an object, for instance the content is an array: `["some", "array"]`,
- There is neither a `request` nor a `response` property in a single-stub mapping file. `{}` or `{ "id": "..." }`,
- There is no stub mapping inside a `mappings` property: `{ "mappings": [ ] }` or `{ "mappings": [ {} ] }`

### Single mapping

A mapping file is considered having a single mapping when:

- There is one request-response mapping in the file with both a request and a response property present:
```json
{
    "request": {
        "method": "PATCH",
        "url": "/patch-url"
    },
    "response": {
      "status": 300
    }
}
```
- There is a `mappings` property, and there is only one stub mapping inside.
That single mapping must have both a request and a response property:
```json
{
    "mappings": [
        {
            "request": {
                "method": "PATCH",
                "url": "/patch-url"
            },
            "response": {
              "status": 300
            }
        }
    ]
}
```

In this case, the file is decorated with the following texts, based on the input values:

| `request.method` | `request.url*`               | `response.status` | Node decoration text               |
|------------------|------------------------------|-------------------|------------------------------------|
| ""               | ""                           | 200               | <no method> <no url> -> 200        |
| ""               | "/url"                       | ""                | <no method> /url -> <no status>    |
| ""               | "/url"                       | 200               | <no method> /url -> 200            |
| PUT              | ""                           | ""                | PUT <no url> -> <no status>        |
| PUT              | ""                           | 200               | PUT <no url> -> 200                |
| PUT              | "/url"                       | ""                | PUT /url -> <no status>            |
| PUT              | "/url"                       | 200               | PUT /url -> 200                    |
| PUT              | "/really-very-very-long-url" | 200               | PUT /really-very-very-lo... -> 200 |

The last example shows that url values are truncated at 20 characters to keep the node decoration concise.

### Multiple mappings

A mapping file is considered having multiple mappings when there is a `mappings` property with more than one stub mapping,
and all mappings contain a `request` and a `response` property inside:
```json
{
    "mappings": [
        {
          "request": { ... }
          "response": { ... },
        },
      {
        "request": {
          "method": "PUT",
          "url": "/put-url"
        },
        "response": {
          "status": 302
        }
      }
    ]
}
```

In this case, the file node is decorated with the text *multiple*.

Where there is a `mappings` property with more than one stub mapping inside,
but at least one of them lacks either the `request` or the `response` property,
it is considered incomplete, and the file node is decorated with *multiple (has incomplete)*. 

### Enable/disable node decoration

There is also a toolbar action in the Project View to enable/disable the node decoration. By default, it is turned off.

It saves the enabled/disable state per project.

![mapping_file_node_decoration_toolbar_action](assets/mapping_file_node_decoration_toolbar_action.png)

### Indexing

The displayed data is coming from a custom index implementation, so if you have the node decoration enabled when the IDE
or a project launches, and you have some mapping folders open, the files inside won't be visible until the IDE finishes indexing.

If you don't use the node decoration feature, it is recommended to disable it to minimize its performance/resource impact on the IDE. 

## Unwrap single stub mapping in multi-mapping definitions

![](https://img.shields.io/badge/inspection-orange) ![](https://img.shields.io/badge/since-1.0.6-blue)

This inspection can report `mappings` properties in JSON mapping files that contain only a single stub mapping.
In that case, having the `mappings` property specified is not necessary, the stub mapping object itself can be the sole
content of the mapping file.

It also provides a quick fix that unwraps the stub mapping, and essentially removes the wrapping around the stub mapping.

Whether that child mapping has any content doesn't matter, the property is reported regardless. However, nothing
is reported when `mappings` has a `meta` sibling property specified, since that one doesn't have a counterpart in
the individual stub mapping schema.

**Example:**

```json
{
  "mappings": [ //"mappings" property name is reported
    {
      "request": { ... },
      "response": { ... }
    }
  ]
}
```

becomes

```json
{
  "request": { ... },
  "response": { ... }
}
```

## Create mapping file from template

![](https://img.shields.io/badge/action-orange) ![](https://img.shields.io/badge/since-1.0.1-blue)

The **Create Mapping File** action is available in the Project view context menu on directories
called `mappings` and on subdirectories of them.
It uses File Templates to pre-populate mapping files with predefined contents.

![create_wiremock_mapping_file_context_menu_action](assets/create_wiremock_mapping_file_context_action.png)

You can select from two templates (both editable in <kbd>Settings</kbd> > <kbd>Editor</kbd> > <kbd>File and Code
Templates</kbd> > <kbd>Files</kbd>).
The provided file name must include the *.json* extension too.

![create_wiremock_mapping_options_dialog](assets/create_wiremock_mapping_options_dialog.PNG)

- single request-response mapping

```json
{
  "request": {
    "method": "GET",
    "url": "/"
  },
  "response": {
    "status": 200
  }
}
```

- multiple request-response mappings

```json
{
  "mappings": [
    {
      "request": {
        "method": "GET",
        "url": "/"
      },
      "response": {
        "status": 200
      }
    },
    {
      "request": {
        "method": "GET",
        "url": "/"
      },
      "response": {
        "status": 200
      }
    }
  ]
}
```

### Split multi-mapping file into multiple single-mapping files

![](https://img.shields.io/badge/action-orange) ![](https://img.shields.io/badge/since-1.0.6-blue)

This action is available in the Project view context menu as *Split Mapping File*. The action is visible regardless of the contents of the mapping files,
but not all mapping files can be/make sense to be split.

The split happens as follows. Given the following mapping file:

```json
# filename: multi_mapping.json
{
  "mappings": [
    {
      "request": { 
        "method": "GET"
      }
      "response": { ... }
    },
    {
      "request": {
        "method": "POST"
      }
      "response": { ... }
    }
  ]
}
```

this action creates two new files:

```json
# filename: multi_mapping_0.json
{
    "request": {
      "method": "GET"
    }
    "response": { ... }
}
```

```json
# filename: multi_mapping_1.json
{
    "request": {
      "method": "POST"
    }
    "response": { ... }
}
```

The filenames are index-postfixes starting from 0. The original mapping file is removed after the new files are created.

Mapping files in certain cases cannot be split. If that is the cases, a notification balloon is displayed on in the Project View,
on the mapping file node, to let you know why the split cannot happen:
- no indexed data for the file (ideally this should not happen),
- there is no mapping in the file
- there is only a single mapping in the file
- there are multiple mappings, but at least one of them doesn't specify either the `request` or `response` property.

### Merge stub mapping files in a selected folder

![](https://img.shields.io/badge/action-orange) ![](https://img.shields.io/badge/since-1.0.6-blue)

This action is available in the Project view context menu as *Merge Mapping Files in This Directory*, when there is at least two
JSON file (be it mapping or non-mapping ones) present in the selected directory.

It merges all JSON mapping files in that directory (files in subdirectories are not included) in three phases, and provides a progress bar as well, in case there is a large
number of files to merge:

- **Phase 1**: collects the stub mapping JSON objects. The mappings are collected in alphabetical order of their containing files' names,
and in the order of their presence in those files. Non-mapping files, and files that don't contain a stub mapping, are excluded. 
- **Phase 2**: merges the collected mappings into a new JSON file, whose name must be specified by the user.
  - NOTE: the *.json* extension must be included when specifying the file name.
- **Phase 3**: deletes all mapping files from the selected directory:
  - NOTE: mapping files that don't contain any mapping, are deleted too. At the moment, there is no option to exclude them from the deletion.

There are popup balloons displayed when the whole merge process, or a part of it, couldn't happen due to some precondition, or an error:
- ***No mapping file to merge.***: when there is no actual mapping file in the directory, even if there are other JSON files present.
- ***A single mapping file won't be merged.***: when there is only a single mapping file in the directory.
- ***Could not get the content of some mapping files. See IDE logs.***: there is a problem reading the content of a file.
- ***Could not delete some mapping files. See IDE logs.***: speaks for itself
- ***Could not merge files. See IDE logs.***: a catch-all message if there was any other kind of issue during any of the phases that is not caught/handled by the merge logic.

When the balloon message includes *See IDE logs*, there is further stacktrace information and reasoning available in the IDE's **idea.log** file.

**An example for the merge logic:**

Given the following two stub mapping files:

*single_mapping.json*
```json
{
  "request": {
    "method": "PATCH",
    "url": "/single-mapping"
  },
  "response": {
    "status": 200,
    "bodyFileName": "a_response_body.json"
  }
}
```

*multi_mapping.json*
```json
{
  "mappings": [
    {
      "request": {
        "method": "GET",
        "url": "/multi-mapping"
      },
      "response": {
        "status": 404,
        "body": "{{systemValue key='aKey' type='PROPERTY'}}"
      }
    },
    {
      "request": {
        "method": "POST",
        "url": "/multi-mapping"
      },
      "response": {
        "status": 500
      }
    }
  ]
}
```

The merged version will be:

```json
{
  "mappings": [
    {
      "request": {
        "method": "GET",
        "url": "/multi-mapping"
      },
      "response": {
        "status": 404,
        "body": "{{systemValue key='aKey' type='PROPERTY'}}"
      }
    },
    {
      "request": {
        "method": "POST",
        "url": "/multi-mapping"
      },
      "response": {
        "status": 500
      }
    },
    {
      "request": {
        "method": "PATCH",
        "url": "/single-mapping"
      },
      "response": {
        "status": 200,
        "bodyFileName": "a_response_body.json"
      }
    }
  ]
}
```