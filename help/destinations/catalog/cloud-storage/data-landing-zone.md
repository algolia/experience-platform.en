---
title: Data Landing Zone destination
description: Learn how to connect to Data Landing Zone to activate audiences and export datasets.
last-substantial-update: 2023-07-26
exl-id: 40b20faa-cce6-41de-81a0-5f15e6c00e64
---
# Data Landing Zone destination

>[!IMPORTANT]
>
>This documentation page refers to the [!DNL Data Landing Zone] *destination*. There is also a [!DNL Data Landing Zone] *source* in the sources catalog. For more information, read the [[!DNL Data Landing Zone] source](/help/sources/connectors/cloud-storage/data-landing-zone.md) documentation.


## Overview {#overview}

[!DNL Data Landing Zone] is a cloud storage interface provisioned by Adobe Experience Platform, granting you access to a secure, cloud-based file storage facility to export files out of Experience Platform. You have access to one [!DNL Data Landing Zone] container per sandbox, and the total data volume across all containers is limited to the total data provided with your Experience Platform Products and Services license. All customers of Experience Platform and its applications such as [!DNL Customer Journey Analytics], [!DNL Journey Orchestration], [!DNL Intelligent Services], and [!DNL Real-Time Customer Data Platform] are provisioned with one [!DNL Data Landing Zone] container per sandbox. 

Experience Platform enforces a strict seven-day time-to-live (TTL) on all files uploaded to a [!DNL Data Landing Zone] container. All files are deleted after seven days.

The [!DNL Data Landing Zone] destination connector is available to customers using the Azure or Amazon Web Service cloud support. The authentication mechanism is different based on the cloud in which the destination is provisioned, everything else about the destination and its use cases are the same. Read more about the two different authentication mechanisms in the sections [Authenticate to the Data Landing Zone provisioned in Azure Blob](#authenticate-dlz-azure) and [Authenticate to the AWS-provisioned Data Landing Zone](#authenticate-dlz-aws).

![Diagram showing how the implementation of the Data Landing Zone destination is different based on the cloud support.](/help/destinations/assets/catalog/cloud-storage/data-landing-zone/dlz-workflow-based-on-cloud-implementation.png "Data Landing Zone destination implementation by cloud support"){zoomable="yes"}

## Connect to your [!UICONTROL Data Landing Zone] storage through API or UI {#connect-api-or-ui}

* To connect to your [!UICONTROL Data Landing Zone] storage location using the Experience Platform user interface, read the sections [Connect to the destination](#connect) and [Activate audiences to this destination](#activate) below.
* To connect to your [!UICONTROL Data Landing Zone] storage location programmatically, read the [Activate audiences to file-based destinations by using the Flow Service API tutorial](../../api/activate-segments-file-based-destinations.md).

## Supported audiences {#supported-audiences}

This section describes which types of audiences you can export to this destination.

| Audience origin | Supported | Description | 
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Audiences generated through the Experience Platform [Segmentation Service](../../../segmentation/home.md).|
| Custom uploads | ✓ | Audiences [imported](../../../segmentation/ui/audience-portal.md#import-audience) into Experience Platform from CSV files. |

{style="table-layout:auto"}

## Export type and frequency {#export-type-frequency}

Refer to the table below for information about the destination export type and frequency.

| Item | Type | Notes |
---------|----------|---------|
| Export type | **[!UICONTROL Profile-based]** | You are exporting all members of a segment, together with the applicable schema fields (for example your PPID), as chosen in the select profile attributes screen of the [destination activation workflow](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes).|
| Export frequency | **[!UICONTROL Batch]** | Batch destinations export files to downstream platforms in increments of three, six, eight, twelve, or twenty-four hours. Read more about [batch file-based destinations](/help/destinations/destination-types.md#file-based).|

{style="table-layout:auto"}

## Export datasets {#export-datasets}

This destination supports dataset exports. For complete information on how to set up dataset exports, read the tutorials: 

* How to [export datasets using the Experience Platform user interface](/help/destinations/ui/export-datasets.md). 
* How to [export datasets programmatically using the Flow Service API](/help/destinations/api/export-datasets.md).

## File format of the exported data {#file-format}

When exporting *audience data*, Experience Platform creates a `.csv`, `parquet`, or `.json` file in the storage location that you provided. For more information about the files, see the [supported file formats for export](../../ui/activate-batch-profile-destinations.md#supported-file-formats-export) section in the audience activation tutorial.

When exporting *datasets*, Experience Platform creates a `.parquet` or `.json` file in the storage location that you provided. For more information about the files, see the [verify successful dataset export](../../ui/export-datasets.md#verify) section in the export datasets tutorial.

## Authenticate to the Data Landing Zone provisioned in Azure Blob {#authenticate-dlz-azure}

>[!AVAILABILITY]
>
>This section applies to implementations of Experience Platform running on Microsoft Azure. To learn more about the supported Experience Platform infrastructure, see the [Experience Platform multi-cloud overview](https://experienceleague.adobe.com/en/docs/experience-platform/landing/multi-cloud).

You can read and write files to your container through [!DNL Azure Storage Explorer] or your command-line interface.

[!DNL Data Landing Zone] supports SAS-based authentication and its data is protected with standard [!DNL Azure Blob] storage security mechanisms at rest and in transit. SAS stands for [shared access signature](https://learn.microsoft.com/en-us/azure/ai-services/translator/document-translation/how-to-guides/create-sas-tokens?tabs=Containers).

To protect your data over a public internet connection, use SAS-based authentication to securely access your [!DNL Data Landing Zone] container. There are no network changes required for you to access your [!DNL Data Landing Zone] container, which means you do not need to configure any allow lists or cross-region setups for your network. 

### Connect your [!DNL Data Landing Zone] container to [!DNL Azure Storage Explorer] 

You can use [[!DNL Azure Storage Explorer]](https://azure.microsoft.com/en-us/products/storage/storage-explorer/) to manage the contents of your [!DNL Data Landing Zone] container. To start using [!DNL Data Landing Zone], you must first retrieve your credentials, input them in [!DNL Azure Storage Explorer], and connect your [!DNL Data Landing Zone] container to [!DNL Azure Storage Explorer].

In the [!DNL Azure Storage Explorer] UI, select the connection icon in the left navigation bar. The **Select Resource** window appears, providing you with options to connect to. Select **[!DNL Blob container]** to connect to your [!DNL Data Landing Zone] storage.

![Select resource highlighted in the Azure UI.](/help/sources/images/tutorials/create/dlz/select-resource.png)

Next, select **Shared access signature URL (SAS)** as your connection method, and then select **Next**.

![Select connection method highlighted in the Azure UI.](/help/sources/images/tutorials/create/dlz/select-connection-method.png)

After selecting your connection method, you must provide a **display name** and the **[!DNL Blob] container SAS URL** that corresponds with your [!DNL Data Landing Zone] container.

>[!BEGINSHADEBOX]

### Retrieve the credentials for your [!DNL Data Landing Zone] {#retrieve-dlz-credentials}

You must use the Experience Platform APIs to retrieve your [!DNL Data Landing Zone] credentials. The API call to retrieve your credentials is described below. For information about getting the required values for your headers, refer the [Getting started with Adobe Experience Platform APIs](/help/landing/api-guide.md) guide.

**API format**

```http
GET /data/foundation/connectors/landingzone/credentials?type=dlz_destination
```

| Query parameters | Description |
| --- | --- |
| `dlz_destination` |  The `dlz_destination` type allows the API to distinguish a landing zone destination container from the other types of containers that are available to you. |

{style="table-layout:auto"}

**Request**

The following request example retrieves credentials for an existing landing zone.

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/connectors/landingzone/credentials?type=dlz_destination' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

**Response**

The following response returns the credential information for your landing zone, including your current `SASToken` and `SASUri`, and the `storageAccountName` that corresponds to your landing zone container.

```json
{
    "containerName": "dlz-destination",
    "SASToken": "sv=2022-09-11&si=dlz-ed86a61d-201f-4b50-b10f-a1bf173066fd&sr=c&sp=racwdlm&sig=4yTba8voU3L0wlcLAv9mZLdZ7NlMahbfYYPTMkQ6ZGU%3D",
    "storageAccountName": "dlblobstore99hh25i3df123",
    "SASUri": "https://dlblobstore99hh25i3dflek.blob.core.windows.net/dlz-destination?sv=2022-09-11&si=dlz-ed86a61d-201f-4b50-b10f-a1bf173066fd&sr=c&sp=racwdlm&sig=4yTba8voU3L0wlcLAv9mZLdZ7NlMahbfYYPTMkQ6ZGU%3D"
}
```

| Property | Description |
| --- | --- |
| `containerName` | The name of your landing zone. |
| `SASToken` | The shared access signature token for your landing zone. This string contains all the information necessary to authorize a request. |
| `SASUri` | The shared access signature URI for your landing zone. This string is a combination of the URI to the landing zone for which you are being authenticated to and its corresponding SAS token, |

{style="table-layout:auto"}

### Update [!DNL Data Landing Zone] credentials {#update-dlz-credentials}

You can also refresh your credentials when desired. You can update your `SASToken` by making a POST request to the `/credentials` endpoint of the [!DNL Connectors] API.

**API format**

```http
POST /data/foundation/connectors/landingzone/credentials?type=dlz_destination&action=refresh
```

| Query parameters | Description |
| --- | --- |
| `dlz_destination` |  The `dlz_destination` type allows the API to distinguish a landing zone destination container from the other types of containers that are available to you. |
| `refresh` | The `refresh` action allows you to reset your landing zone credentials and automatically generate a new `SASToken`. |

{style="table-layout:auto"}

**Request**

The following request updates your landing zone credentials.

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/connectors/landingzone/credentials?type=dlz_destination&action=refresh' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

**Response**

The following response returns updated values for your `SASToken` and `SASUri`.

```json
{
    "containerName": "dlz-destination",
    "SASToken": "sv=2020-04-08&si=dlz-9c4d03b8-a6ff-41be-9dcf-20123e717e99&sr=c&sp=racwdlm&sig=JbRMoDmFHQU4OWOpgrKdbZ1d%2BkvslO35%2FXTqBO%2FgbRA%3D",
    "storageAccountName": "dlblobstore99hh25i3dflek",
    "SASUri": "https://dlblobstore99hh25i3dflek.blob.core.windows.net/dlz-destination?sv=2020-04-08&si=dlz-9c4d03b8-a6ff-41be-9dcf-20123e717e99&sr=c&sp=racwdlm&sig=JbRMoDmFHQU4OWOpgrKdbZ1d%2BkvslO35%2FXTqBO%2FgbRA%3D"
}
```

>[!ENDSHADEBOX]

Provide your display name (`containerName`) and [!DNL Data Landing Zone] SAS URL, as returned in the API call described above, and then select **Next**.

![Enter connection info highlighted in the Azure UI.](/help/sources/images/tutorials/create/dlz/enter-connection-info.png)

The **Summary** window appears, providing you with an overview of your settings, including information on your [!DNL Blob] endpoint and permissions. When ready, select **Connect**.

![Summary of settings shown in the Azure UI.](/help/sources/images/tutorials/create/dlz/summary.png)

A successful connection updates your [!DNL Azure Storage Explorer] UI with your [!DNL Data Landing Zone] container.

![Summary of the DLZ user container highlighted in the Azure UI.](/help/sources/images/tutorials/create/dlz/dlz-user-container.png)

With your [!DNL Data Landing Zone] container connected to [!DNL Azure Storage Explorer], you can now start exporting files from Experience Platform to your [!DNL Data Landing Zone] container. To export files, you must establish a connection to the [!DNL Data Landing Zone] destination in the Experience Platform UI, as described in the section below. 

## Authenticate to the AWS-provisioned Data Landing Zone {#authenticate-dlz-aws}

>[!AVAILABILITY]
>
>This section applies to implementations of Experience Platform running on Amazon Web Services (AWS). Experience Platform running on AWS is currently available to a limited number of customers. To learn more about the supported Experience Platform infrastructure, see the [Experience Platform multi-cloud overview](https://experienceleague.adobe.com/en/docs/experience-platform/landing/multi-cloud).

Perform the operations below to get credentials to your [!DNL Data Landing Zone] instance provisioned on AWS. Then, use a client of choice to connect to your [!DNL Data Landing Zone] instance. 

>[!BEGINSHADEBOX]

### Retrieve the credentials for your [!DNL Data Landing Zone] {#retrieve-dlz-credentials-aws}

You must use the Experience Platform APIs to retrieve your [!DNL Data Landing Zone] credentials. The API call to retrieve your credentials is described below. For information about getting the required values for your headers, refer the [Getting started with Adobe Experience Platform APIs](/help/landing/api-guide.md) guide.

**API format**

```http
GET /data/foundation/connectors/landingzone/credentials?type=dlz_destination'
```

| Query parameters | Description |
| --- | --- |
| `dlz_destination` |  Add the `dlz_destination` query parameter to specify that you want the [!DNL Data Landing Zone] *destination* type of container credentials to be retrieved. To connect and retrieve credentials for a Data Landing Zone *source*, view the [sources documentation](/help/sources/connectors/cloud-storage/data-landing-zone.md). |

{style="table-layout:auto"}

**Request**

The following request example retrieves credentials for an existing landing zone.

```shell
curl --request GET \
  --url 'https://platform.adobe.io/data/foundation/connectors/landingzone/credentials?type=dlz_destination' \
  --header 'Authorization: Bearer ***' \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: your_api_key' \
  --header 'x-gw-ims-org-id: yourorg@AdobeOrg'
```

**Response**

The following response returns the credential information for your landing zone, including your current `awsAccessKeyId`, `awsSecretAccessKey`, and other information.

```json
{
    "credentials": {
        "awsAccessKeyId": "ABCDW3MEC6HE2T73ZVKP",
        "awsSecretAccessKey": "A1B2Zdxj6y4xfR0QZGtf/phj/hNMAbOGtzM/JNeE",
        "awsSessionToken": "***"
    },
    "dlzPath": {
        "bucketName": "your-bucket-name",
        "dlzFolder": "dlz-destination"
    },
    "dlzProvider": "Amazon S3",
    "expiryTime": 1734494017
}
```

| Property | Description |
| --- | --- |
| `credentials` | This object includes the `awsAccessKeyId`, `awsSecretAccessKey`, and `awsSessionToken` that Experience Platform uses to export files to your provisioned Data Landing Zone location. |
| `dlzPath` | This object includes the path in the Adobe-provisioned AWS location where exported files are deposited. |
| `dlzProvider` | Indicates that this is an Amazon S3-provisioned Data Landing Zone. |
| `expiryTime` | Indicates when the credentials in the `credentials` object will expire. To refresh the credetials, perform the request again. |

{style="table-layout:auto"}

>[!ENDSHADEBOX]

## Connect to the destination {#connect}

>[!IMPORTANT]
> 
>To connect to the destination, you need the **[!UICONTROL View Destinations]** and **[!UICONTROL Manage Destinations]** [access control permissions](/help/access-control/home.md#permissions). Read the [access control overview](/help/access-control/ui/overview.md) or contact your product administrator to obtain the required permissions.

To connect to this destination, follow the steps described in the [destination configuration tutorial](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html). In the destination configuration workflow, fill in the fields listed in the two sections below.

### Authenticate to destination {#authenticate}

Make sure that you have connected your [!DNL Data Landing Zone] container to [!DNL Azure Storage Explorer] as described in the [prerequisites](#prerequisites) section. Because [!DNL Data Landing Zone] is an Adobe-provisioned storage, you do not need to perform any further steps in the Experience Platform UI to authenticate to the destination.

### Fill in destination details {#destination-details}

To configure details for the destination, fill in the required and optional fields below. An asterisk next to a field in the UI indicates that the field is required.

*  **[!UICONTROL Name]**: Fill in the preferred name for this destination.
*  **[!UICONTROL Description]**: Optional. For example, you can mention which campaign you are using this destination for.
* **[!UICONTROL Folder path]**: Enter the path to the destination folder that will host the exported files.
* **[!UICONTROL File type]**: Select the format Experience Platform should use for the exported files. When selecting the [!UICONTROL CSV] option, you can also [configure the file formatting options](../../ui/batch-destinations-file-formatting-options.md).
* **[!UICONTROL Compression format]**: Select the compression type that Experience Platform should use for the exported files.
* **[!UICONTROL Include manifest file]**: Toggle this option on if you'd like the exports to include a manifest JSON file that contains information about the export location, export size, and more. The manifest is named using the format `manifest-<<destinationId>>-<<dataflowRunId>>.json`. View a [sample manifest file](/help/destinations/assets/common/manifest-d0420d72-756c-4159-9e7f-7d3e2f8b501e-0ac8f3c0-29bd-40aa-82c1-f1b7e0657b19.json). The manifest file includes the following fields:
  * `flowRunId`: The [dataflow run](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations) which generated the exported file. 
  * `scheduledTime`: The time in UTC when the file was exported. 
  * `exportResults.sinkPath`: The path in your storage location where the exported file is deposited. 
  * `exportResults.name`: The name of the exported file.
  * `size`: The size of the exported file, in bytes.

### Enable alerts {#enable-alerts}

You can enable alerts to receive notifications on the status of the dataflow to your destination. Select an alert from the list to subscribe to receive notifications on the status of your dataflow. For more information on alerts, see the guide on [subscribing to destinations alerts using the UI](../../ui/alerts.md).

When you are finished providing details for your destination connection, select **[!UICONTROL Next]**.

## Activate audiences to this destination {#activate}

>[!IMPORTANT]
> 
>* To activate data, you need the **[!UICONTROL View Destinations]**, **[!UICONTROL Activate Destinations]**, **[!UICONTROL View Profiles]**, and **[!UICONTROL View Segments]** [access control permissions](/help/access-control/home.md#permissions). Read the [access control overview](/help/access-control/ui/overview.md) or contact your product administrator to obtain the required permissions.
>* To export *identities*, you need the **[!UICONTROL View Identity Graph]** [access control permission](/help/access-control/home.md#permissions). <br> ![Select identity namespace highlighted in the workflow to activate audiences to destinations.](/help/destinations/assets/overview/export-identities-to-destination.png "Select identity namespace highlighted in the workflow to activate audiences to destinations."){width="100" zoomable="yes"}

See [Activate audience data to batch profile export destinations](../../ui/activate-batch-profile-destinations.md) for instructions on activating audiences to this destination.

### Scheduling

In the **[!UICONTROL Scheduling]** step, you can [set up the export schedule](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling) for your [!DNL Data Landing Zone] destination and you can also [configure the name of your exported files](/help/destinations/ui/activate-batch-profile-destinations.md#file-names). 

### Map attributes and identities {#map}

In the **[!UICONTROL Mapping]** step, you can select which attribute and identity fields to export for your profiles. You can also select to change the headers in the exported file to any friendly name that you wish. For more information, view the [mapping step](/help/destinations/ui/activate-batch-profile-destinations.md#mapping) in the activate batch destinations UI tutorial.

## Validate successful data export {#exported-data}

To verify if data has been exported successfully, check your [!DNL Data Landing Zone] storage and make sure that the exported files contain the expected profile populations.
