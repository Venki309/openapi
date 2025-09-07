# Converting OpenAPI YAML Files into HTML Documentation

This guide walks you through the process of turning individual and merged OpenAPI YAML files into neat, interactive HTML documentation using **Redocly**.

## Prerequisites

Before you start,  install the following

- **Node.js** (latest recommended)  
- **Redocly CLI** (`npm install -g @redocly/cli`)  
- **openapi-merge-cli** (`npm install -g openapi-merge-cli`)  

## Steps

Follow these steps to convert the single or multiple YAML files to a clean HTML file.

1. [Review API Definitions](#review-api-definitions)  
2. [Convert Individual YAML Files to HTML](#convert-individual-yaml-files-to-html)  
3. [Create a merge.json File](#create-a-mergejson-file)  
4. [Merge the API Files](#merge-the-api-files)  
5. [Organize the Table of Contents](#organize-the-table-of-contents)  
6. [Build the Final HTML](#build-the-final-html)  

## Review API Definitions

Review each OpenAPI YAML file to ensure accuracy:

- Verify that summaries, descriptions, and response messages are grammatically correct and clear.  
- Confirm that each file strictly follows the **OpenAPI specification**. Otherwise, Redocly will throw validation errors.

**Note:** Run `redocly lint my-api.yaml` to catch specification issues early.  

Check the [OpenAPI repository](https://github.com/Venki309/openapi) for demo source files.

## Convert Individual YAML Files to HTML

Convert each YAML file to HTML to validate the format and content:

```bash
redocly build-docs pet-api.yaml -o pet-api.html
redocly build-docs aquarium-api.yaml -o aquarium-api.html
```
Repeat this for every source YAML file to check for errors and fix them before continuing.

## Create a merge.json File

To merge multiple API definitions, create a [merge.json](https://github.com/Venki309/openapi/blob/main/openapi-merge.json) configuration file.

Example:

```bash
{
  "inputs": [
    {
        "inputFile": "pet/pet-api.yaml",
        "pathModification": {
            "prepend": "/api"
        }
    },
    {
        "inputFile": "aquarium/aquarium-api.yaml",
        "pathModification": {
            "prepend": "/api"
        }
    }
    ],
"output": "merged-api.yaml"
}
```

## Merge the API Files

Run the following merge command to generate a combined OpenAPI YAML file.
```
npx openapi-merge-cli
```

See the [merged-api.yaml](https://github.com/Venki309/openapi/blob/main/merged-api.yaml) for reference.

## Organize the Table of Contents

After merging, structure your documentation for easy navigation:

- Add x-tagGroups in the merged YAML to group related endpoints under meaningful headings.
    
    Example:

    ```bash
    x-tagGroups:
  - name: Pet Management
    tags:
      - Pet Information
      - Pet Adoption
      - Pet Health
  - name: Aquarium Management
    tags:
      - Fish Care
      - Water Quality
      - Equipment
    ```

- Copy the **responses** and **schemas** folders (and their contents) into the same directory as the merged-api.yaml file. This ensures that references resolve correctly, else you might get errors while converting to HTML file.

## Build the Final HTML

Run the following command to generate a single HTML file from the merged YAML file.

```
redocly build-docs input-file.yaml -o output-file.html
```

You now have a clean, consolidated [HTML](https://venki309.github.io/openapi/unified-api.html) view of all your APIs.


