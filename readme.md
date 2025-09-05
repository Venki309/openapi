# Converting OpenAPI YAML Files into HTML Documentation

This guide walks you through the process of turning individual and merged OpenAPI YAML files into neat, interactive HTML documentation using **Redocly**.

## Prerequisites

Before you start, make sure you have the following installed on your system:

- **Node.js** (latest LTS recommended)  
- **Redocly CLI** (`npm install -g @redocly/cli`)  
- **openapi-merge-cli** (`npm install -g openapi-merge-cli`)  


## Review API Definitions

Carefully check each OpenAPI YAML file:

- Ensure summaries, descriptions, and response messages are grammatically correct and clear.  
- Confirm that each file strictly follows the **OpenAPI specification**. Otherwise, Redocly will throw validation errors.

**Note:** Run `redocly lint my-api.yaml` to catch specification issues early.  

Check [OpenAPI repository](https://github.com/Venki309/openapi) for demo source files.

## Convert Individual YAML Files to HTML

For each API file, run the Redocly build command:

```bash
redocly build-docs pet-api.yaml -o pet-api.html
redocly build-docs aquarium-api.yaml -o aquarium-api.html
```
Repeat this for every source YAML file to check for errors.

## Create a merge.json File

To merge multiple API definitions, you need a [merge.json](https://github.com/Venki309/openapi/blob/main/openapi-merge.json) configuration file.

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

Run the merge command to generate a combined OpenAPI file:

```
npx openapi-merge-cli --config merge.json
```

This produces a single [merged-api.yaml](https://github.com/Venki309/openapi/blob/main/merged-api.yaml).

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

- Copy the **responses** and **schemas** folders (and their contents) next to the merged-api.yaml file. This ensures that references resolve correctly, else you might get errors while converting to HTML file.

## Build the Final HTML

Finally, convert the merged YAML file into a single HTML documentation.

```
redocly build-docs input-file.yaml -o output-file.html
```

You now have a clean, consolidated [HTML](https://venki309.github.io/openapi/) view of all your APIs.


