---
description: This page exemplifies the API call used to update an existing credential configuration through Adobe Experience Platform Destination SDK.
title: Update a credential configuration
exl-id: ebff370c-9189-48df-871f-ed0e1cd535c8
---
# Update a credential configuration

>[!IMPORTANT]
>
>**API endpoint**: `platform.adobe.io/data/core/activation/authoring/credentials`

This page exemplifies the API request and payload that you can use to update an existing credential configuration, using the `/authoring/credentials` API endpoint.

## When to use the `/credentials` API endpoint {#when-to-use}

>[!IMPORTANT]
>
>In most cases, you ***do not*** need to use the `/credentials` API endpoint. Instead, you can configure the authentication information for your destination via the `customerAuthenticationConfigurations` parameters of the `/destinations` endpoint.
> 
>Read [Customer authentication configuration](../functionality/destination-configuration/customer-authentication.md) for detailed information on the supported authentication types.

Use this API endpoint to create a credential configuration only if there is a global authentication system between Adobe and your destination platform, and the [!DNL Experience Platform] customer does not need to provide any authentication credentials to connect to your destination. In this case, you must create a credential configuration using the `/credentials` API endpoint.

When using a global authentication system, you must set `"authenticationRule":"PLATFORM_AUTHENTICATION"` in the [destination delivery](../functionality/destination-configuration/destination-delivery.md) configuration, when [creating a new destination configuration](../authoring-api/destination-configuration/create-destination-configuration.md).

>[!IMPORTANT]
>
>All parameter names and values supported by Destination SDK are **case sensitive**. To avoid case sensitivity errors, please use the parameters names and values exactly as shown in the documentation.

## Getting started with credentials API operations {#get-started}

Before continuing, please review the [getting started guide](../getting-started.md) for important information that you need to know in order to successfully make calls to the API, including how to obtain the required destination authoring permission and required headers.

## Update a credential configuration {#update}

You can update an [existing](create-credential-configuration.md) credential configuration by making a `PUT` request to the `/authoring/credentials` endpoint with the updated payload.

To obtain an existing credential configuration and its corresponding `{INSTANCE_ID}`, see the article about [retrieving a credential configuration](retrieve-credential-configuration.md).

**API format**

```http
PUT /authoring/credentials/{INSTANCE_ID}
```

| Parameter | Description |
| -------- | ----------- |
| `{INSTANCE_ID}` | The ID of the credential configuration that you want to update. To obtain an existing credential configuration and its corresponding `{INSTANCE_ID}`, see [Retrieve a credential configuration](retrieve-credential-configuration.md).|

The following requests update existing credential configurations, defined by the parameters provided in the payload.

Select each tab below to view the corresponding payload.

>[!BEGINTABS]

>[!TAB Basic]

**Update a basic credential configuration**

+++Request

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/credentials/{INSTANCE_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
    "basicAuthentication":{
      "url":"string",
      "username":"string",
      "password":"string"
   }
}
```

| Parameter | Type | Description |
| -------- | ----------- | ----------- |
|`url` | String | URL of authorization provider |
|`username` | String | Credentials configuration login username |
|`password` | String | Credentials configuration login password |

{style="table-layout:auto"}

+++

+++Response

A successful response returns HTTP status 200 with the details of your updated credential configuration.

+++

>[!TAB Amazon S3]

**Update an [!DNL Amazon S3] credential configuration**

+++Request

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/credentials/{INSTANCE_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "s3Authentication":{
      "accessId":"string",
      "secretKey":"string"
   }
}
```

| Parameter | Type | Description |
| -------- | ----------- | ----------- |
|`accessId`|String|[!DNL Amazon S3] access ID|
|`secretKey`|String|[!DNL Amazon S3] secret key|

{style="table-layout:auto"}

+++

+++Response

A successful response returns HTTP status 200 with the details of your updated credential configuration.

+++

>[!TAB SSH]

**Update an [!DNL SSH] credential configuration**

+++Request

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/credentials/{INSTANCE_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
    "sshAuthentication":{
      "username":"string",
      "sshKey":"string"
   }
}
```

| Parameter | Type | Description |
| -------- | ----------- | ----------- |
|`username` | String | Credentials configuration login username |
|`sshKey`|String|[!DNL SSH] key for [!DNL SFTP] with [!DNL SSH] authentication|

{style="table-layout:auto"}

+++

+++Response

A successful response returns HTTP status 200 with the details of your updated credential configuration.

+++

>[!TAB Azure Data Lake Storage]

**Update an [!DNL Azure Data Lake Storage] credential configuration**

+++Request

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/credentials/{INSTANCE_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
    "azureAuthentication":{
      "url":"string",
      "tenant":"string",
      "servicePrincipalId":"string",
      "servicePrincipalKey":"string"
   }
}
```

| Parameter | Type | Description |
| -------- | ----------- | ----------- |
|`url` | String | URL of authorization provider |
|`tenant`|String|Azure Data Lake Storage tenant|
|`servicePrincipalId`|String|[!DNL Azure Service Principal] ID for [!DNL Azure Data Lake Storage]|
|`servicePrincipalKey`|String|[!DNL Azure Service Principal Key] for [!DNL Azure Data Lake Storage]|

{style="table-layout:auto"}

+++

+++Response

A successful response returns HTTP status 200 with the details of your updated credential configuration.

+++

>[!TAB Azure Blob Storage]

**Update an [!DNL Azure Blob] credential configuration**

+++Request

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/credentials/{INSTANCE_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
    "azureConnectionStringAuthentication":{
      "connectionString":"string"
   }
}
```

| Parameter | Type | Description |
| -------- | ----------- | ----------- |
|`connectionString`|String|[!DNL Azure Blob Storage] connection string|

{style="table-layout:auto"}

+++

+++Response

A successful response returns HTTP status 200 with the details of your updated credential configuration.

+++

>[!ENDTABS]

## API error handling {#error-handling}

Destination SDK API endpoints follow the general Experience Platform API error message principles. Refer to [API status codes](../../../landing/troubleshooting.md#api-status-codes) and [request header errors](../../../landing/troubleshooting.md#request-header-errors) in the Experience Platform troubleshooting guide.

## Next steps {#next-steps}

After reading this document, you now know how to update a credential configuration using the `/authoring/credentials` API endpoint. Read [how to use Destination SDK to configure your destination](../guides/configure-destination-instructions.md) to understand where this step fits into the process of configuring your destination.
