---
solution: Experience Platform
title: Salesforce Marketing Cloud Source Overview
description: Learn how to connect Salesforce Marketing Cloud to Adobe Experience Platform using APIs or the user interface.
exl-id: 2177d68c-0cef-4031-a0e7-8bf22ee2e70b
last-substantial-update: 2025-04-29
---
# [!DNL Salesforce Marketing Cloud]

>[!WARNING]
>
>The [!DNL Salesforce Marketing Cloud] source will be deprecated in January 2026. A new source will be released later this year as an alternative. Once the new source is released, you must plan to migrate to the new source by creating new account connections and dataflows before the end of January 2026.

Adobe Experience Platform allows data to be ingested from external sources while providing you with the ability to structure, label, and enhance incoming data using Experience Platform services. You can ingest data from a variety of sources such as Adobe applications, cloud-based storage, databases, and many others.

Experience Platform provides support for ingesting data from third-party marketing automation systems. Support for marketing automation providers include [!DNL Salesforce Marketing Cloud].

## Prerequisites

Before you can connect your [!DNL Salesforce Marketing Cloud] source to Experience Platform, you must ensure that the following **permission scopes** are provisioned to your [!DNL Salesforce Marketing Cloud] client ID and client secret combination:

* `campaign_read`
* `list_and_subscribers_read`

You can request for scopes by making a call to the `v2/userinfo` resource of the [!DNL Salesforce Marketing Cloud] API. See the [[!DNL Salesforce Marketing Cloud] API Integration Permission Scopes document](<https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/data-access-permissions.html>) for guidance on how to request and compare scopes.

For more information on scopes including a list of their related permissions and behaviors, see this [[!DNL Salesforce Marketing Cloud] REST API document](<https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/rest-permissions-and-scopes.html>).

>[!IMPORTANT]
>
>Custom object ingestion is currently not supported by the [!DNL Salesforce Marketing Cloud] source integration.

## IP address allowlist

You must add region-specific IP addresses to your allowlist prior to connecting your sources to Experience Platform. For more information, read the guide on [allowlisting IP addresses to connect to Experience Platform](../../ip-address-allow-list.md) for more information.

>[!WARNING]
>
>If you do not add the necessary IP addresses to your allowlist, your [!DNL Salesforce Marketing Cloud] account will not connect to Experience Platform.

## Connect [!DNL Salesforce Marketing Cloud] to Experience Platform using APIs

The documentation below provides information on how to connect [!DNL Salesforce Marketing Cloud] to Experience Platform using APIs:

* [Create a Salesforce Marketing Cloud base connection using the Flow Service API](../../tutorials/api/create/marketing-automation/salesforce-marketing-cloud.md)
* [Explore data tables using the Flow Service API](../../tutorials/api/explore/tabular.md)
* [Create a dataflow for a marketing automation source using the Flow Service API](../../tutorials/api/collect/marketing-automation.md)

## Connect [!DNL Salesforce Marketing Cloud] to Experience Platform using the UI

The documentation below provides information on how to connect [!DNL Salesforce Marketing Cloud] to Experience Platform using the user interface:

* [Create a Salesforce Marketing Cloud source connection in the UI](../../tutorials/ui/create/marketing-automation/salesforce-marketing-cloud.md)
* [Create a dataflow for a marketing automation source connection in the UI](../../tutorials/ui/dataflow/marketing-automation.md)
