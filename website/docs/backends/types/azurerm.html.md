---
layout: "backend-types"
page_title: "Backend Type: azurerm"
sidebar_current: "docs-backends-types-standard-azurerm"
description: |-
  Terraform can store state remotely in Azure Blob Storage.

---

# azurerm

**Kind: Standard (with state locking)**

Stores the state as a Blob with the given Key within the Blob Container within [the Blob Storage Account](https://docs.microsoft.com/azure/storage/common/storage-introduction). This backend also supports state locking and consistency checking via native capabilities of Azure Blob Storage.

## Example Configuration

When authenticating using a Service Principal:

```hcl
terraform {
  backend "azurerm" {
    storage_account_name = "abcd1234"
    container_name       = "tfstate"
    key                  = "prod.terraform.tfstate"
  }
}
```

When authenticating using the Access Key associated with the Storage Account:

```hcl
terraform {
  backend "azurerm" {
    storage_account_name = "abcd1234"
    container_name       = "tfstate"
    key                  = "prod.terraform.tfstate"
    access_key           = "abcdefghijklmnopqrstuvwxyz0123456789..."
  }
}
```

-> **NOTE:** When using a Service Principal or an Access Key - we recommend using a [Partial Configuration](/docs/backends/config.html) for the credentials.

## Example Referencing

When authenticating using a Service Principal:

```hcl
data "terraform_remote_state" "foo" {
  backend = "azurerm"
  config = {
    storage_account_name = "terraform123abc"
    container_name       = "terraform-state"
    key                  = "prod.terraform.tfstate"
  }
}
```

When authenticating using the Access Key associated with the Storage Account:

```hcl
data "terraform_remote_state" "foo" {
  backend = "azurerm"
  config = {
    storage_account_name = "terraform123abc"
    container_name       = "terraform-state"
    key                  = "prod.terraform.tfstate"
    access_key           = "abcdefghijklmnopqrstuvwxyz0123456789..."
  }
}
```

## Configuration variables

The following configuration options are supported:

* `storage_account_name` - (Required) The Name of [the Storage Account](https://www.terraform.io/docs/providers/azurerm/r/storage_account.html).

* `container_name` - (Required) The Name of [the Storage Container](https://www.terraform.io/docs/providers/azurerm/r/storage_container.html) within the Storage Account.

* `key` - (Required) The name of the Blob used to retrieve/store Terraform's State file inside the Storage Container.

* `environment` - (Optional) The Azure Environment which should be used. This can also be sourced from the `ARM_ENVIRONMENT` environment variable. Possible values are `public`, `china`, `german`, `stack` and `usgovernment`. Defaults to `public`.

---

When authenticating using the Storage Account's Access Key - the following fields are also supported:

* `access_key` - (Optional) The Access Key used to access the Blob Storage Account. This can also be sourced from the `ARM_ACCESS_KEY` environment variable.

---

When authenticating using a Service Principal - the following fields are also supported:

* `resource_group_name` - (Required) The Name of the Resource Group in which the Storage Account exists.

* `arm_client_id` - (Optional) The Client ID of the Service Principal. This can also be sourced from the `ARM_CLIENT_ID` environment variable.

* `arm_client_secret` - (Optional) The Client Secret of the Service Principal. This can also be sourced from the `ARM_CLIENT_SECRET` environment variable.

* `arm_subscription_id` - (Optional) The Subscription ID in which the Storage Account exists. This can also be sourced from the `ARM_SUBSCRIPTION_ID` environment variable.

* `arm_tenant_id` - (Optional) The Tenant ID in which the Subscription exists. This can also be sourced from the `ARM_TENANT_ID` environment variable.
