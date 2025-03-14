---
subcategory: "Databricks SQL"
---
# databricks_sql_dashboard Resource

This resource is used to manage [Databricks SQL Dashboards](https://docs.databricks.com/sql/user/dashboards/index.html). To manage [SQLA resources](https://docs.databricks.com/sql/get-started/concepts.html) you must have `databricks_sql_access` on your [databricks_group](group.md#databricks_sql_access) or [databricks_user](user.md#databricks_sql_access).

**Note:** documentation for this resource is a work in progress.

A dashboard may have one or more [widgets](sql_widget.md).

## Example Usage

```hcl
resource "databricks_sql_dashboard" "d1" {
  name = "My Dashboard Name"

  tags = [
    "some-tag",
    "another-tag",
  ]
}
```

Example [permission](permissions.md) to share dashboard with all users:

```hcl
resource "databricks_permissions" "d1" {
  sql_dashboard_id = databricks_sql_dashboard.d1.id

  access_control {
    group_name       = data.databricks_group.users.display_name
    permission_level = "CAN_RUN"
  }
}
```

## Import

-> **Note** Importing this resource is not currently supported.

## Related Resources

The following resources are often used in the same context:

* [End to end workspace management](../guides/workspace-management.md) guide.
* [databricks_sql_endpoint](sql_endpoint.md) to manage Databricks SQL [Endpoints](https://docs.databricks.com/sql/admin/sql-endpoints.html).
* [databricks_sql_global_config](sql_global_config.md) to configure the security policy, [databricks_instance_profile](instance_profile.md), and [data access properties](https://docs.databricks.com/sql/admin/data-access-configuration.html) for all [databricks_sql_endpoint](sql_endpoint.md) of workspace.
* [databricks_sql_permissions](sql_permissions.md) to manage data object access control lists in Databricks workspaces for things like tables, views, databases, and [more](https://docs.databricks.com/security/access-control/table-acls/object-privileges.html).
