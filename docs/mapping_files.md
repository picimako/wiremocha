# Mapping files

![](https://img.shields.io/badge/schema-orange) ![](https://img.shields.io/badge/since-1.0.3-blue)

A JSON schema is bound to JSON mapping files, which automatically provides validation and code completion.
It works both for single- and multi-mapping files.

The schema is based on the original WireMock schemas in
the [WireMock GitHub repository](https://github.com/wiremock/wiremock/tree/master/src/main/resources/swagger/schemas).

NOTE: the validation and auto-completion features are not implemented in this plugin, but in the IntelliJ platform
itself.
It simply provides the configuration for the IntelliJ platform, so that it knows what conditions must a file meet to
assign a certain schema file to it.

### Create mapping file from template

![](https://img.shields.io/badge/action-orange) ![](https://img.shields.io/badge/since-1.0.1-blue)

The **Create WireMock Mapping File** action is available in the Project view context menu on directories
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