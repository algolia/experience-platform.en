---
keywords: Experience Platform;home;popular topics;dataset api;manage data usage;data usage api
solution: Experience Platform
title: Manage Data Usage Labels for Datasets Using APIs 
description: The Dataset Service API allows you to apply and edit usage labels for datasets. It is part of Adobe Experience Platform's data catalog capabilities, but is separate from the Catalog Service API which manages dataset metadata.
exl-id: 24a8d870-eb81-4255-8e47-09ae7ad7a721
---
# Manage data usage labels for datasets using APIs

The [[!DNL Dataset Service API]](https://www.adobe.io/experience-platform-apis/references/dataset-service/) allows you to apply and edit usage labels for datasets. It is part of Adobe Experience Platform's data catalog capabilities, but is separate from the [!DNL Catalog Service] API which manages dataset metadata.

>[!IMPORTANT]
>
>Applying labels at the dataset level is only supported for data governance use cases. If you are trying to create access policies for the data, you must [apply labels to the schema](../../xdm/tutorials/labels.md) that the dataset is based on. See the overview on [attribute-based access control](../../access-control/abac/overview.md) for more information.

This document covers how to manage labels for datasets and fields using the [!DNL Dataset Service API]. For steps on how to manage data usage labels themselves using API calls, see the [labels endpoint guide](../api/labels.md) for the [!DNL Policy Service API].

## Getting started

Before you read this guide, follow the steps outlined in the [getting started section](../../catalog/api/getting-started.md) in the Catalog developer guide to gather the required credentials to make calls to [!DNL Experience Platform] APIs.

In order to make calls to the endpoints outlined in this document, you must have the unique `id` value for a specific dataset. If you do not have this value, see the guide on [listing Catalog objects](../../catalog/api/list-objects.md) to find the IDs of your existing datasets.

## Look up labels for a dataset {#look-up}

You can look up the data usage labels that have been applied to an existing dataset by making a GET request to the [!DNL Dataset Service] API.

**API format**

```http
GET /datasets/{DATASET_ID}/labels
```

| Parameter | Description |
| --- | --- |
| `{DATASET_ID}` | The unique `id` value of the dataset whose labels you want to look up. |

**Request**

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/dataset/datasets/5abd49645591445e1ba04f87/labels' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**Response**

A successful response returns the data usage labels that have been applied to the dataset.

```json
{
  "AEP:dataset:5abd49645591445e1ba04f87": {
    "imsOrg": "{ORG_ID}",
    "labels": [ "C1", "C2", "C3", "I1", "I2" ],
    "optionalLabels": [
      {
        "option": {
          "id": "https://ns.adobe.com/{TENANT_ID}/schemas/c6b1b09bc3f2ad2627c1ecc719826836",
          "contentType": "application/vnd.adobe.xed-full+json;version=1",
          "schemaPath": "/properties/repositoryCreatedBy"
        },
        "labels": [ "S1", "S2" ]
      }
    ]
  }
}
```

| Property | Description |
| --- | --- |
| `labels` | A list of data usage labels that have been applied to the dataset. |
| `optionalLabels` | A list of individual fields within the dataset that have data usage labels applied to them. |

## Apply labels to a dataset {#apply}

You can apply a set of labels for an entire dataset by providing them in the payload of a POST or PUT request to the [!DNL Dataset Service] API. The request body is the same for both calls. You cannot add labels to individual dataset fields.   

**API format**

```http
POST /datasets/{DATASET_ID}/labels
PUT /datasets/{DATASET_ID}/labels
```

| Parameter | Description |
| --- | --- |
| `{DATASET_ID}` | The unique `id` value of the dataset that you are creating labels for. |

**Request**

The example POST request below updates the entire dataset with a `C1` label. The fields provided in the payload are the same as would be required for a PUT request.

When making API calls that update the existing labels of a dataset (PUT), an `If-Match` header that indicates the current version of the dataset-label entity in Dataset Service must be included. In order to prevent data collisions, the service will only update the dataset entity if the included `If-Match` string matches the latest version tag generated by the system for that dataset.

>[!NOTE]
>
>If labels currently exist for the dataset in question, new labels can only be added through a PUT request, which requires an `If-Match` header. Once labels have been added to a dataset, the most recent `etag` value is required to update or remove the labels at a later time<br>Before executing the PUT method, you must perform a GET request on the dataset labels. Ensure that you update only the specific fields intended for modification in the request, leaving the rest unchanged. Additionally, make sure that the PUT call maintains the same parent entities as the GET call. Any discrepancy would result in an error for the customer.

To retrieve the most recent version of the dataset-label entity, make a [GET request](#look-up) to the `/datasets/{DATASET_ID}/labels` endpoint. The current value is returned in the response under an `etag` header. When updating existing dataset labels, best practice is to first perform a lookup request for the dataset in order to fetch its latest `etag` value before using that value in the `If-Match` header of your subsequent PUT request.

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/dataset/datasets/5abd49645591445e1ba04f87/labels' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "entityId": {
          "namespace": "AEP",
          "id": "test-ds-id",
          "type": "dataset"
        },
        "labels": [
          "C1"
        ],
        "parents": []
      } '
```

| Property | Description |
| --- | --- |
| `entityId` | This identifies the specific dataset entity to be updated. |
| `entityId.namespace` | This is used to avoid ID collisions. The `namespace` is `AEP`. | 
| `entityId.id` | The ID of the resource being updated. This refers to the `datasetId`. | 
| `entityId.type` | The type of the resource being updated. This will always be `dataset`.| 
| `labels` | A list of data usage labels that you want to add to the entire dataset. |
| `parents` | The `parents` array contains a list of `entityId`s that this dataset will inherit labels from. Datasets can inherit labels from schemas and/or datasets. |

**Response**

A successful response returns the updated set of labels for the dataset.

>[!IMPORTANT]
>
>The `optionalLabels` property has been deprecated for use with POST requests. It is no longer possible to add data labels to dataset fields. A POST operation will throw an error if an `optionalLabel` value is present. However, you can delete labels from individual fields using a PUT request and the `optionalLabels` property. For more information see the section on [removing labels from a dataset](#remove).

```json
{
  "parents": [
      {
        "id": "_ddgduleint.schemas.4a95cdba7d560e3bca7d8c5c7b58f00ca543e2bb1e4137d6",
        "type": "schema",
        "namespace": "AEP"
      }
  ],
  "optionalLabels": [],
  "labels": [
      "C1"
  ],
  "code": "PES-201",
  "message": "POST Successful"
} 
```

## Remove labels from a dataset {#remove}

You can remove any previously applied field labels by either updating the existing `optionalLabels` value(s) with a subset of the existing field labels, or an empty list to remove them entirely. Make a PUT request to the [!DNL Dataset Service] API to update or remove previously applied labels.

>[!NOTE]
>
>You can entirely remove a dataset's labels by providing an empty list for the `labels` parameter. It is not mandatory for a dataset to retain any labels.

**API format**

```http
PUT /datasets/{DATASET_ID}/labels
```

| Parameter | Description |
| --- | --- |
| `{DATASET_ID}` | The unique `id` value of the dataset that you are creating labels for. |

**Request**

The below dataset on which PUT operation is applied had C1 optionalLabel on properties/person/properties/address field and C1, C2 optionalLabels on /properties/person/properties/name/properties/fullName field. After the put operation, first field will have no label (C1 label got removed) and second field will have only C1 label (C2 label got removed)

In the example scenario below, a PUT request is used to remove labels aded to individual fields. Before the request was made, the `fullName` field had the `C1` and `C2` labels applied, and the `address` field already had a `C1` label applied. The PUT request overrides existing labels `C1, C2` labels from the `fullName` field with a `C1` label using the `optionalLabels.labels` parameter. The request also overrides the `C1` label from the `address` field with an empty set of field labels.

```shell
curl -X PUT \
  'https://platform.adobe.io/data/foundation/dataset/datasets/5abd49645591445e1ba04f87/labels' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -H 'If-Match: 8f00d38e-0000-0200-0000-5ef4fc6d0000' \
  -d '{
        "entityId": {
          "namespace": "AEP",
          "id": "646b814d52e1691c07b41032",
          "type": "dataset"
        },
        "labels": [
          "C1"
        ],
        "parents": [
          {
            "id": "_xdm.context.identity-graph-flattened-export",
            "type": "schema",
            "namespace": "AEP"
          }
        ],
        "optionalLabels": [
          {
            "option": {
              "id": "https://ns.adobe.com/xdm/context/identity-graph-flattened-export",
              "contentType": "application/vnd.adobe.xed-full+json;version=1",
              "schemaPath": "/properties/person/properties/name/properties/fullName"
            },
            "labels": [
              "C1"
            ]
          },
          {
            "option": {
              "id": "https://ns.adobe.com/xdm/context/identity-graph-flattened-export",
              "contentType": "application/vnd.adobe.xed-full+json;version=1",
              "schemaPath": "/properties/person/properties/address"
            },
            "labels": []
          }
        ]
      }'
```

| Parameter | Description |
| --- | --- |
| `entityId` | This identifies the specific dataset entity to be updated. The `entityId` must include the following three values:<br/><br/>`namespace`: This is used to avoid ID collisions. The `namespace` is `AEP`.<br/>`id`: The ID of the resource being updated. This refers to the `datasetId`.<br/>`type`: The type of the resource being updated. This will always be `dataset`. | 
| `labels` | A list of data usage labels that you want to add to the entire dataset. |
| `parents` | The `parents` array contains a list of `entityId`s that this dataset will inherit labels from. Datasets can inherit labels from schemas and/or datasets. |
| `optionalLabels` | This parameter is used to remove labels previously applied to a dataset field. A list of any individual fields within the dataset that you want to remove the labels from. Each item in this array must have the following properties: <br/><br/>`option`: An object that contains the [!DNL Experience Data Model] (XDM) attributes of the field. The following three properties are required:<ul><li><code>id</code>: The URI <code>$id</code> value of the schema associated with the field.</li><li><code>contentType</code>: The content type and version number of the schema. This should take the form of one of the valid <a href="../../xdm/api/getting-started.md#accept">Accept headers</a> for an XDM lookup request.</li><li><code>schemaPath</code>: The path to the field within the dataset's schema.</li></ul>`labels`: This value must contain either a subset of the existing field labels applied, or be empty to remove all the existing field labels. PUT or POST methods now return an error if the `optionalLabels` field has any new or modified labels. |

**Response**

A successful response returns the updated set of labels for the dataset.

```json
{
  "parents": [
      {
        "id": "_xdm.context.identity-graph-flattened-export",
        "type": "schema",
        "namespace": "AEP"
      }
  ],
  "optionalLabels": [],
  "labels": [
      "C1"
  ],
  "code": "PES-200",
  "message": "PUT Successful"
} 
```

## Next steps

By reading this document, you have learned how to manage data usage labels for datasets and fields using the [!DNL Dataset Service] API. You can now define [data usage policies](../policies/overview.md) and [access control policies](../../access-control/abac/ui/policies.md) based on the labels you have applied.

For more information on managing datasets in [!DNL Experience Platform], see the [datasets overview](../../catalog/datasets/overview.md).
