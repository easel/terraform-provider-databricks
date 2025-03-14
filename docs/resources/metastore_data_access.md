---
subcategory: "Unity Catalog"
---
# databricks_metastore_data_access (Resource)

-> **Public Preview** This feature is in [Public Preview](https://docs.databricks.com/release-notes/release-types.html). Contact your Databricks representative to request access. 

Each [databricks_metastore](docs/resources/metastore.md) requires an IAM role that will be assumed by Unity Catalog to access data. `databricks_metastore_data_access` defines this

## Example Usage

For AWS

```hcl
resource "databricks_metastore" "this" {
  name          = "primary"
  storage_root  = "s3://${aws_s3_bucket.metastore.id}/metastore"
  owner         = "uc admins"
  force_destroy = true
}

resource "databricks_metastore_data_access" "this" {
  metastore_id = databricks_metastore.this.id
  name         = aws_iam_role.metastore_data_access.name
  aws_iam_role {
    role_arn = aws_iam_role.metastore_data_access.arn
  }
  is_default = true
}
```

For Azure

```hcl
resource "databricks_metastore" "this" {
  name = "primary"
  storage_root = format("abfss://%s@%s.dfs.core.windows.net/",
    azurerm_storage_account.unity_catalog.name,
  azurerm_storage_container.unity_catalog.name)
  owner         = "uc admins"
  force_destroy = true
}

resource "databricks_metastore_data_access" "this" {
  metastore_id = databricks_metastore.this.id
  name         = aws_iam_role.metastore_data_access.name
  azure_service_principal {
    directory_id   = var.tenant_id
    application_id = azuread_application.unity_catalog.application_id
    client_secret  = azuread_application_password.unity_catalog.value
  }
  is_default = true
}
```

## Argument Reference

The following arguments are required:

* `name` - Name of Data Access Configuration, which must be unique within the [databricks_metastore](metastore.md). Change forces creation of a new resource.
* `metastore_id` - Unique identifier of the parent Metastore

`aws_iam_role` optional configuration block for credential details for AWS:
* `role_arn` - The Amazon Resource Name (ARN) of the AWS IAM role for S3 data access, of the form `arn:aws:iam::1234567890:role/MyRole-AJJHDSKSDF`

`azure_service_principal` optional configuration block for credential details for Azure:
* `directory_id` - The directory ID corresponding to the Azure Active Directory (AAD) tenant of the application
* `application_id` - The application ID of the application registration within the referenced AAD tenant
* `client_secret` - The client secret generated for the above app ID in AAD. **This field is redacted on output**

## Import

-> **Note** Importing this resource is not currently supported.
