---
title: Create a Microsoft SQL Server Base Connection Using the Flow Service API
type: Tutorial
description: Learn how to connect Adobe Experience Platform to a Microsoft SQL Server using the Flow Service API.
exl-id: 00455a61-c8c1-42f4-a962-fc16f7370cbd
---
# Create a [!DNL Microsoft] SQL Server base connection using the [!DNL Flow Service] API

A base connection represents the authenticated connection between a source and Adobe Experience Platform.

Read this tutorial to learn how to create a base connection for [!DNL Microsoft SQL Server] using the [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## Get started

This guide requires a working understanding of the following components of Adobe Experience Platform:

* [Sources](../../../../home.md): Experience Platform allows data to be ingested from various sources while providing you with the ability to structure, label, and enhance incoming data using Experience Platform services.
* [Sandboxes](../../../../../sandboxes/home.md): Experience Platform provides virtual sandboxes which partition a single Experience Platform instance into separate virtual environments to help develop and evolve digital experience applications.

The following sections provide additional information that you will need to know in order to successfully connect to [!DNL Microsoft SQL Server] using the [!DNL Flow Service] API.

### Gather required credentials {#gather-required-credentials}

In order to connect to [!DNL Microsoft SQL Server], you must provide the following connection property:

| Credential | Description | Example |
| --- | --- | --- |
| `connectionString` | The connection string associated with your [!DNL Microsoft SQL Server] account. Your connection string pattern depends whether you are using server name or instance name for your data source:<ul><li>Connection string using server name: `Data Source={SERVER_NAME};Initial Catalog={DATABASE};Integrated Security=False;User ID={USER_ID};Password={PASSWORD};`</li><li>Connection string using instance name:`Data Source={INSTANCE_NAME};Initial Catalog={DATABASE};Integrated Security=False;User ID={USER_ID};Password={PASSWORD};` | `Data Source=mssqlserver.database.windows.net;Initial Catalog=mssqlserver_e2e_db;Integrated Security=False;User ID=mssqluser;Password=mssqlpassword` |
| `connectionSpec.id` | The connection specification returns a source's connector properties, including authentication specifications related to creating the base and source connections. The connection specification ID for [!DNL Microsoft SQL Server] is `1f372ff9-38a4-4492-96f5-b9a4e4bd00ec`. |

For more information about obtaining a connection string, refer to this [[!DNL Microsoft SQL Server] document](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/sql/authentication-in-sql-server).

### Using Experience Platform APIs

For information on how to successfully make calls to Experience Platform APIs, see the guide on [getting started with Experience Platform APIs](../../../../../landing/api-guide.md).

## Create a base connection

A base connection retains information between your source and Experience Platform, including your source's authentication credentials, the current state of the connection, and your unique base connection ID. The base connection ID allows you to explore and navigate files from within your source and identify the specific items that you want to ingest, including information regarding their data types and formats.

To create a base connection ID, make a POST request to the `/connections` endpoint while providing your [!DNL Microsoft SQL Server] authentication credentials as part of the request parameters.

**API format**

```https
POST /connections
```

**Request**

The following request creates a base connection for [!DNL Microsoft SQL Server]:

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Base connection for sql-server",
      "description": "Base connection for sql-server",
      "auth": {
          "specName": "Connection String Based Authentication",
          "params": {
              "connectionString": "Data Source=mssqlserver.database.windows.net;Initial Catalog=mssqlserver_e2e_db;Integrated Security=False;User ID=mssqluser;Password=mssqlpassword"
          }
      },
      "connectionSpec": {
          "id": "1f372ff9-38a4-4492-96f5-b9a4e4bd00ec",
          "version": "1.0"
  }'
```

| Property | Description |
| --- | --- |
| `auth.params.connectionString` | The connection string associated with your [!DNL Microsoft SQL Server] account. Read the section on [gathering required credentials](#gather-required-credentials) for more information. |
| `connectionSpec.id` | The [!DNL Microsoft SQL Server] connection specification ID is: `1f372ff9-38a4-4492-96f5-b9a4e4bd00ec`. |

**Response**

A successful response returns details of the newly created connection, including its unique identifier (`id`). This ID is required to explore your database in the next tutorial.

```json
{
    "id": "0b8224e4-0de8-4293-8224-e40de80293c6",
    "etag": "\"5802c519-0000-0200-0000-5e4d89520000\""
}
```

## Next steps

By following this tutorial, you have created an [!DNL Microsoft SQL Server] base connection using the [!DNL Flow Service] API. You can use this base connection ID in the following tutorials:

* [Explore the structure and contents of your data tables using the [!DNL Flow Service] API](../../explore/tabular.md)
* [Create a dataflow to bring database data to Experience Platform using the [!DNL Flow Service] API](../../collect/database-nosql.md)