---
keywords: email;Email;e-mail;email destinations;oracle eloqua;oracle
title: (Files) Oracle Eloqua connection
description: Oracle Eloqua is a software as a service (SaaS) platform for marketing automation offered by Oracle that aims to help B2B marketers and organizations manage marketing campaigns and sales lead generation.
exl-id: 6eaa79ff-8874-423b-bdff-aa04f6101a53
---
# [!DNL (Files) Oracle Eloqua] connection

[[!DNL Oracle Eloqua]](https://www.oracle.com/cx/marketing/automation/) is a software as a service (SaaS) platform for marketing automation offered by [!DNL Oracle] that aims to help B2B marketers and organizations manage marketing campaigns and sales lead generation. 

To send audience data to [!DNL Oracle Eloqua], you must first [connect the destination](#connect-destination) in Adobe Experience Platform, and then [set up a data import](#import-data-into-eloqua) from your storage location into [!DNL Oracle Eloqua].

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
| Export type | **[!UICONTROL Profile-based]** | You are exporting all members of a segment, together with the desired schema fields (for example: email address, phone number, last name), as chosen in the select profile attributes screen of the [destination activation workflow](../../ui/activate-batch-profile-destinations.md#select-attributes).|
| Export frequency | **[!UICONTROL Batch]** | Batch destinations export files to downstream platforms in increments of three, six, eight, twelve, or twenty-four hours. Read more about [batch file-based destinations](/help/destinations/destination-types.md#file-based).|

{style="table-layout:auto"}

## IP address allow list {#allow-list}

When setting up email marketing destinations with SFTP storage, Adobe recommends that you add certain IP ranges to your allow list.

Refer to [IP address allow list for SFTP destinations](../cloud-storage/ip-address-allow-list.md) if you need to add Adobe IPs to your allow list.

## Connect to the destination {#connect}

>[!IMPORTANT]
> 
>To connect to the destination, you need the **[!UICONTROL Manage Destinations]** [access control permission](/help/access-control/home.md#permissions). Read the [access control overview](/help/access-control/ui/overview.md) or contact your product administrator to obtain the required permissions

To connect to this destination, follow the steps described in the [destination configuration tutorial](../../ui/connect-destination.md).

This destination supports the following connection types:

* **[!UICONTROL SFTP with Password]**
* **[!UICONTROL SFTP with SSH Key]**

### Connection parameters {#parameters}

While [setting up](../../ui/connect-destination.md) this destination, you must provide the following information:

* For **[!UICONTROL SFTP with Password]** connections, you must provide:
  * [!UICONTROL Domain]
  * [!UICONTROL Port]
  * [!UICONTROL Username]
  * [!UICONTROL Password]
* For **[!UICONTROL SFTP with SSH Key]** connections, you must provide:
  * [!UICONTROL Domain]
  * [!UICONTROL Port]
  * [!UICONTROL Username]
  * [!UICONTROL SSH Key]

* Optionally, you can attach your RSA-formatted public key to add encryption with PGP/GPG to your exported files under the **[!UICONTROL Key]** section. Your public key must be written as a [!DNL Base64] encoded string.
* **[!UICONTROL Name]**: Pick a relevant name for your destination.
* **[!UICONTROL Description]**: Enter a description for your destination.
* **[!UICONTROL Folder Path]**: Provide the path in your storage location where Experience Platform will deposit your export data as CSV files.
* **[!UICONTROL File Format]**: Select **CSV** to export CSV files to your storage location.

<!--

Commenting out Amazon S3 bucket part for now until support is clarified

- **[!UICONTROL Bucket name]**: Your Amazon S3 bucket, where Experience Platform will deposit the data export. Your input must be between 3 and 63 characters long. Must begin and end with a letter or number. Must contain only lowercase letters, numbers, or hyphens ( - ). Must not be formatted as an IP address (for example, 192.100.1.1).

-->

### Enable alerts {#enable-alerts}

You can enable alerts to receive notifications on the status of the dataflow to your destination. Select an alert from the list to subscribe to receive notifications on the status of your dataflow. For more information on alerts, see the guide on [subscribing to destinations alerts using the UI](../../ui/alerts.md).

When you are finished providing details for your destination connection, select **[!UICONTROL Next]**.

## Activate audiences to this destination {#activate}

>[!IMPORTANT]
> 
>* To activate data, you need the **[!UICONTROL View Destinations]**, **[!UICONTROL Activate Destinations]**, **[!UICONTROL View Profiles]**, and **[!UICONTROL View Segments]** [access control permissions](/help/access-control/home.md#permissions). Read the [access control overview](/help/access-control/ui/overview.md) or contact your product administrator to obtain the required permissions.
>* To export *identities*, you need the **[!UICONTROL View Identity Graph]** [access control permission](/help/access-control/home.md#permissions). <br> ![Select identity namespace highlighted in the workflow to activate audiences to destinations.](/help/destinations/assets/overview/export-identities-to-destination.png "Select identity namespace highlighted in the workflow to activate audiences to destinations."){width="100" zoomable="yes"}

See [Activate audience data to batch profile export destinations](../../ui/activate-batch-profile-destinations.md) for instructions on activating audiences to this destination.

### Destination attributes {#destination-attributes}

When activating audiences to this destination, Adobe recommends that you select a unique identifier from your [union schema](../../../profile/home.md#profile-fragments-and-union-schemas). Select the unique identifier and any other XDM fields that you want to export to the destination. For more information, refer to [best practices when activating audiences to email marketing destinations](overview.md#best-practices).

## Exported data {#exported-data}

For [!DNL Oracle Eloqua] destinations, Experience Platform creates a `.csv` file in the storage location that you provided. For more information about the files, see [verify audience activation](../../ui/activate-batch-profile-destinations.md#verify) in the audience activation tutorial. 

## Set up data import into [!DNL Oracle Eloqua] {#import-data-into-eloqua}

After connecting [!DNL Experience Platform] to your [!DNL SFTP] storage, you must set up the data import from your storage location into [!DNL Oracle Eloqua]. To learn how to accomplish this, see [Importing contacts or accounts](https://docs.oracle.com/cloud/latest/marketingcs_gs/OMCAA/Help/DataImportExport/Tasks/ImportingContactsOrAccounts.htm) in the [!DNL Oracle Eloqua Help Center].
