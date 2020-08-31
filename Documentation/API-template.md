---
uid: ocsAPItemplate
---

*** Add a line before each API 

## `API_name` 
Description of API. Use present tense, "Creates XXX", for example. 

### Request 
```text 

GET{POST, PUT, DELETE} api/version/Resource1/{resource1Id}/Resource2/{resource2Id}  

``` 

### Parameters
`datatype resource1Id`   
resource1Id description. `resourceId` should be camelCase. 

[Optional] `datatype resource2Id`   
[Optional] resource2Id description. No need to tag the parameter if it is required. 

If there is more than one type of parameter, specify each one. Use H3 header (three hashtags). See examples below.
```text
Examples
### Request path parameter
### Request query parameters
```

#### Request body 
Description of request body
 
#### Example request body 
This section is optional. 
```json 
 Put code example here. 
``` 

### Authorization
Admin API: Specify the roles authorized to access the resource or execute the API method.  
Consumer API: Use the text below
```text
Access to the resources/collection is controlled by ACL (Access Control List) and Owner objects.
For more information, see [Role-based access control](https://ocs-docs.osisoft.com/Content_Portal/Documentation/Access_Control.html).
```
### Response 
Add API method response.  Enter a description and a type, mention body as well. Use a table to present responses.
Code example can follow. 

| Status Code | Response Type | Description | 
|--|--|--| 
| 200 OK | `Object` | Object description | 
| 403 Forbidden | error | You are not authorized for this operation | 
| 404 Not Found | error | The data view or query does not exist | 
| 500 Internal Server Error | error | An error occurred while processing the request. | 

```json 
HTTP 200 OK 
Put code example here. 
``` 

#### Response headers 
 This section is optional. Add any important information about headers. 

Successful (200 OK) responses include one or more header values related to paging. 
| Header | Description | 
|--|--| 
| Header1 | Description of Header1 | 
| Header2 | Description of Header2 | 

#### Response body 
Description of response body

#### Example response body
This section is optional.
```json 
Put code example here. 
``` 

*** Add a line before each API. 
To see APIs formatted according to the template, go to [Get Stream](https://ocs-docs.osisoft.com/Content_Portal/Documentation/SequentialDataStore/SDS_Streams.html#get-stream)
or [Get Data Views](https://ocs-docs.osisoft.com/Content_Portal/Documentation/DataViews/DataViewsAPIOverview/Data_View_API.html#get-data-views)
