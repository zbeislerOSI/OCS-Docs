---
uid: AssetsAPI
---

# Assets API

The Assets API allows a user to create, read, update, and delete assets. 

See Access Control API, Asset Centric API, and Assets Search API for additional details. 

The asset feature in OCS supports the HTTP entity tag (ETag) and If-Match for conditional requests. When a GET call is performed, the HTTP response header will contain an Etag which tells the user what version of the asset resource was retrieved.

Example: This is version 7 of this particular asset.
``` 
Etag : "7"
``` 

If the user wants to edit or delete this particular asset, specify If-Match in the HTTP request header when calling DELETE or PUT:

Example: Modify or delete only if the current asset version matches version 7. Do not perform this operation otherwise.  If this condition fails, a 412 wil be returned.
``` 
If-Match : "7"
``` 

Note that If-Match is optional and if the user wants to delete or modify an asset to what is specified in the request body regardless of version, do not specify an If-Match.

## `Get Asset by Id` 
Returns the specified asset along with the version Etag in the HTTP response header. 

### Request 
``` 
GET api/v1-preview/Tenants/{tenantId}/Namespaces/{namespaceId}/Assets/{assetId}  
```

### Parameters  
`string tenantId` 

The tenant identifier

`string namespaceId` 

The namespace identifier

`string assetId`

The asset identifier

### Response 

The response includes a status code and a response body. 

| Status Code | Body Type | Description |
|--|--|--|
| 200 OK | `Asset` | The requested asset. |
| 400 Bad Request | error | The request is not valid. See the response body for additional details. |
| 403 Forbidden | error | You are not authorized to view the requested asset. |
| 404 Not Found | error | The asset with the specified identifier is not found. |

#### Example response body
```json 
HTTP 200 OK 
Content-Type: application/json
{
    "Id": "Heater_01_01_02",
    "Name": "HeaterOnFirstFloor",
    "Description": "This is Asset which represents a heater on the first floor.",
    "Metadata": [
        {
            "Id": "17020d80-1dc8-4690-932f-3421c9cff0d1",
            "Name": "ModelNumber",
            "Description": "This is attribute with double value representing the model number.",
            "SdsTypeCode": 14,
            "Value": 1.3
        }
    ],
    "StreamReferences": [
        {
            "Id": "63c0ba1d-f2db-4b28-b650-7e45afca9ab4",
            "Name": "Data",
            "Description": "This is reference to a stream. The stream has data coming from a heater.",
            "StreamId": "PI_bifrostbigdaddy_1"
        }
    ]
}
```
***

## `Get Assets` 
Returns an array of assets. 

### Request 
```
GET api/v1-preview/Tenants/{tenantId}/Namespaces/{namespaceId}/Assets?skip={skip}&count={count} 
```

### Parameters  
`string tenantId` 

The tenant identifier

`string namespaceId` 

The namespace identifier

[optional] `int skip` 

An optional parameter representing the zero-based offset of the first asset to retrieve. If not specified, a default value of 0 is used.

[optional] `int count` 

An optional parameter representing the maximum number of assets to retrieve. If not specified, a default value of 100 is used. 

### Response 
The response includes a status code and a body. 

| Status Code | Body Type | Description |
|--|--|--|
| 200 OK | `Asset[]` | A page of assets. A response header, `Total-Count`, indicates the total size of the collection. |
| 204 No Content | none | No assets were found or the user does not have permission to view assets. |
| 400 Bad Request | error | The request is not valid. See the response body for additional details. |
| 503 Service Unavailable | error | An error occurred while processing the request. See the response body for additional details. |

***

## `Create Asset` 
Create a new asset with a specified Id. 

If the asset the user is trying to create references an asset type (via the AssetTypeId property) and if there is the corresponding asset type has a metadata value with the same id, then the name and sds type code of the metadata value on the asset must be null. If the asset type does not have metadata value with a corresponding id, name and sds type code may not be null.

### Request 
```text 
POST api/v1-preview/Tenants/{tenantId}/Namespaces/{namespaceId}/Assets/{assetId} 
```

### Parameters  
`string tenantId` 

The tenant identifier

`string namespaceId` 

The namespace identifier

`string assetId`

The asset identifier


#### Request body 
An `asset` object

#### Example request body 
NOTE: To create an asset with a specific Id, use the API route with Id. If this is used, the user must specify a matching Id field for the asset object in the JSON object below.

```json 
{
    "Name": "HeaterOnFirstFloor",
    "Description": "This is Asset which represents a heater on the first floor.",
    "Metadata": [
        {
            "Id": "c0e29698-d157-4288-9dea-db290de1fb35",
            "Name": "ModelNumber",
            "Description": "This is attribute with double value representing the model number.",
            "SdsTypeCode": 14,
            "Value": 1.3
        }
    ],
    "StreamReferences": [
        {
            "Id": "a1b1c2cc-0a1d-4d89-b53e-e7db746bb4d2",
            "Name": "Data",
            "Description": "This is reference to a stream. The stream has data coming from a heater.",
            "StreamId": "heaterData_1"
        }
    ]
}
```
### Response 

The response includes a status code, a body as well as the Etag version in the HTTP response header.

| Status Code               | Body Type | Description                                     |
| ------------------------- | --------- | ----------------------------------------------- |
| 200 OK                    | `Asset`  | The asset as persisted, including values for optional parameters that were omitted in the request.                           |
| 400 Bad Request           | error     | The request is not valid. See the response body for additional details.      |
| 403 Forbidden            | error     | You are not authorized to create assets.           |
| 409 Conflict | error     | The asset create has a conflict. See the response body for additional details. |

***

## `Create Assets (Bulk create)` 

Create multiple assets in a single call.

### Request 

```text 
POST api/v1-preview/Tenants/{tenantId}/Namespaces/{namespaceId}/Assets   

```

### Parameters  

`string tenantId` 

The tenant identifier

`string namespaceId` 

The namespace identifier

#### Request body 

An array of `asset` objects

### Response 

The response includes a status code and a body. 

| Status Code               | Body Type | Description                                     |
| ------------------------- | --------- | ----------------------------------------------- |
| 200 OK                    | `Asset[]`  | An array of assets as persisted, including values for optional parameters that were omitted in the request.                               |
| 400 Bad Request             | error     | The request is not valid. The response will include which asset failed validation checks. See the response body for additional details.      |
| 403 Forbidden            | error     | You are not authorized to create assets.           |
| 409 Conflict | error     | The asset create has a conflict. See the response body for additional details.  |

***

## `Create or Update Asset` 

Create or update an asset with a specified Id.  If asset already exists, the user may specify an If-Match propety in the HTTP request header to ensure that they are modifying the asset only if the version matches. 

### Request 

```text 
PUT api/v1-preview/Tenants/{tenantId}/Namespaces/{namespaceId}/Assets/{assetId} 

```

### Parameters  

.`string tenantId` 

The tenant identifier

`string namespaceId` 

The namespace identifier

`string assetId`

The asset identifier

#### Request body 

The newly created or updated `asset` object.

#### Asset Type Concordance

If an asset type Id is specified for an asset, then the following is true:
- The stream references name of an asset is set to null if the stream reference Id matches the stream reference Id of the asset type.
- The name of a metadata value is set to null for those metadata values whose Ids match the asset type metadata value Ids.

### Response 

The response includes a status code, a body as well as the Etag version in the HTTP response header.

| Status Code              | Body Type | Description                                     |
| -------------------------| --------- | ----------------------------------------------- |
| 200 OK                   | `Asset`  | The newly created or updated asset as persisted, including values for optional parameters that were omitted in the request.                               |
| 400 Bad Request          | error     | The request is not valid. The response will include which asset failed validation checks. See the response body for additional details.      |
| 403 Forbidden            | error     | You are not authorized to update assets. |
| 404 Not Found            | error     | The asset, with the specified identifier, was not found.            |
| 409 Conflict             | error     | The asset update or create has a conflict. See the response body for additional details. |
| 412 Pre-Condition Failed | error     | The asset failed to update due to If-Match condition failing. |

***

## `Delete Asset` 

Delete an asset with a specified Id. The user may specify an If-Match propety in the HTTP request header to ensure that they are deleting the asset only if the version matches.

### Request 

```text 
DELETE api/v1-preview/Tenants/{tenantId}/Namespaces/{namespaceId}/Assets/{assetId}   

```

### Parameters  

`string tenantId` 

The tenant identifier

`string namespaceId` 

The namespace identifier

`string assetId`

The asset identifier

#### Request body 

None

### Response 

The response includes a status code and a body.

| Status Code               | Body Type | Description                                     |
| ------------------------- | --------- | ----------------------------------------------- |
| 204 No Content            | none  | The asset with the specified Id is deleted.                              |
| 400 Bad Request           | error     | The request is not valid. The response will include which asset failed validation checks. See the response body for additional details.       |
| 403 Forbidden             | error     | You are not authorized to delete this asset.       |
| 404 Not Found             | error     | The asset with the specified Id could not be found. 
| 412 Pre-Condition Failed  | error     | The asset failed to update due to If-Match condition failing. |
