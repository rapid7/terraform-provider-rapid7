---
page_title: "Bulk Imports"
---

# Bulk Imports

Refer to the [Terraform documentation](https://developer.hashicorp.com/terraform/language/import/bulk) for a general overview of bulk importing.

For organizations adopting DaC, custom rules manually created via the user interface can be brought under Terraform management by importing them:

1. Create a [query file](https://developer.hashicorp.com/terraform/language/files/tfquery) in your Terraform configuration to define a query specifying which custom rules will be imported. For example, in `rules.tfquery.hcl`:

    ```hcl
    list "rapid7_siem_detection_rule" "rules" {
        provider = rapid7
        config {
            # An optional filtering block can be specified to limit rules returned.
            # See the detection rule listing API documentation for available filters
            # filtering {
            #     filter {
            #         target = "name"
            #         operator = "ILIKE"
            #         value = "test"
            #     }
            # }
        }
    }
    ```

2. Run Terraform to execute the query, and generate Terraform configuration for the matching rules:

    ```bash
    terraform query -generate-config-out imports.tf
    ```

    The command will also output the names and identifiers of rules matching the query.

3. Inspect the generated configuration file, and move the `resource "rapid7_siem_detection_rule" "..." {` blocks and their corresponding `import {` blocks for all rules you would like to import to an appropriate location in your Terraform configuration.

    The resource labels can also be renamed from their auto-generated names at this stage, as long as the corresponding `to` field in the associated `import {` block is also updated.

    Once all needed resources have been organized into your existing Terraform configuration the auto-generated configuration file should be deleted.

4. Plan, review and apply.

    Planning and applying of large bulk imports benefit from improved efficiency by passing the `-parallelism` flag to Terraform to make use of batched API calls. The detection rules API supports reading of up to 100 rules in a single API call when passing `-parallelism=100`

5. Once importing is completed, the `import {` blocks can be removed from your configuration