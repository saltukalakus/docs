---
addon: Azure Blob Storage
title: Azure Blob Storage Add-on
thirdParty: true
public: false
url: /addons/azure-blob-storage
alias:
  - azure blob storage
  - azblob
image: /media/platforms/azure.png
topics:
  - quickstart
  - azure
  - addons
articles:
  - authenticate
contentType: how-to
description: Learn how to use Auth0 to authenticate and authorize Azure Blob Storage.
useCase: integrate-third-party-apps
---

# Azure Blob Storage Add-on

<%= include('../_includes/_uses-delegation') %>

Here's a sample call to the delegation endpoint to get the Shared Access Signature (SAS):

```text
POST https://${account.namespace}/delegation
Content-Type: 'application/json'
{
  "client_id":   "${account.clientId}",
  "grant_type":  "urn:ietf:params:oauth:grant-type:jwt-bearer",
  "id_token":    "YOUR_ID_TOKEN",
  "target":      "${account.clientId}",
  "api_type":    "azure_blob",
  "scope":       "openid"
}
```

* The `client_id` value identifies the requesting app (such as your website) and `YOUR_ID_TOKEN` identifies the user you are requesting this on behalf-of. (Notice that the `id_token` is signed with the `client_id` corresponding `clientSecret`).
* The `target` parameter identifies this API endpoint in Auth0 (often the same as `${account.clientId}`. This is the `client_id` of the app where this add-on has been enabled.
* `api_type` must be `azure_blob`.
* `scope` must be `openid`.

The result of calling the delegation endpoint will be something like:

```json
{
  "azure_blob_sas": "st=2015-01-08T18%3A45%3A14Z&se=2015-01-08T18%3A50%3A14Z&sp=r&sv=2014-02-14&sr=b&sig=13ABC456..."
}
```

You can use the blob SAS token either by appending it to a URL directly or by passing it to one of the Azure Storage SDKs.

```text
GET https://{STORAGEACCOUNT}.blob.core.windows.net/mycontainer/myblob.txt?st=2015-01-08T18%3A45%3A14Z&se=2015-01-08T18%3A50%3A14Z&sp=r&sv=2014-02-14&sr=b&sig=13ABC456...
```
