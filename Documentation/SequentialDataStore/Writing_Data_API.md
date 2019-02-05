---
uid: sdsWritingDataApi
---

# API calls for writing data
  
*****

## `Insert Values`

Inserts data into the specified stream. Returns an error if data is already present at the index of any event.

**Request**

        POST api/v1-preview/Tenants/{tenantId}/Namespaces/{namespaceId}/Streams/{streamId}/Data

**Parameters**  
``string tenantId``  
The tenant identifier  
  
``string namespaceId``  
The namespace identifier  
  
``string streamId``  
The stream identifier  

**Request Body**
A serialized list of one or more events of the stream type

**Response**  
The response includes a status code

**_Notes_**  
This request will return an error if an event already exists for any index in the request. If any individual index encounters a problem, the entire operation is rolled back and no insertions are made. The streamId and index that caused the issue are included in the error response.

The events to be inserted must be formatted as a serialized JSON array of the stream's type. JSON arrays are comma-delimited lists of a type enclosed within square brackets. The following code shows a list of three WaveData events that are properly formatted for insertion. See the [OCS-Samples](https://github.com/osisoft/OCS-Samples) for the complete example.

You can serialize your data using one of many available JSON serializers available at [Introducing JSON](http://json.org/index.html). 

For HTTP requests, the events to be inserted must be formatted as a serialized JSON array of type ``T``. JSON arrays are comma-delimited lists of type ``T`` enclosed within square brackets. The following code shows a list  of three WaveData events that are properly formatted for insertion. See the SDS code samples for the complete WaveData example.
```json
[
    {
        "Order":2,
        "Tau":0.25722883666666846,
        "Radians":1.6162164471269089,
        "Sin":1.9979373673043652,
        "Cos":-0.090809010174665111,
        "Tan":-44.003064529862513,
        "Sinh":4.8353589272389,
        "Cosh":5.2326566823391856,
        "Tanh":1.8481468289554672
    }, 
    {
        "Order":4,
        "Tau":0.25724560000002383,
        "Radians":1.6163217742567466,
        "Sin":1.9979277915696148,
        "Cos":-0.091019446679060964,
        "Tan":-43.901119254534827,
        "Sinh":4.8359100947709592,
        "Cosh":5.233166005842703,
        "Tanh":1.8481776000882766
    }, 
    {
        "Order":6,
        "Tau":0.25724560000002383,
        "Radians":1.6163217742567466,
        "Sin":1.9979277915696148,
        "Cos":-0.091019446679060964,
        "Tan":-43.901119254534827,
        "Sinh":4.8359100947709592,
        "Cosh":5.233166005842703,
        "Tanh":1.8481776000882766
    }
]
```

**.NET Library**
```csharp
    Task InsertValueAsync<T>(string streamId, T item);
    Task InsertValuesAsync<T>(string streamId, IList<T> items);
```

**********************

## `Patch Values`

Modifies the specified stream event(s). Patching affects only the data item parameters that are included in the call.

**Request**

       PATCH api/v1-preview/Tenants/{tenantId}/Namespaces/{namespaceId}/Streams/{streamId}/Data
		    ?select={selectExpression}

**Parameters**  
``string tenantId``  
The tenant identifier
  
``string namespaceId``  
The namespace identifier  
  
``string streamId``  
The stream identifier 
  
``string selectExpression``  
CSV list of strings that indicates the event fields that will be changed in stream events.  

**Request Body**  
A serialized list of one or more patch property events

**Response**  
The response includes a status code

**_Notes_**  
Patching is used to patch the events of the selected fields for one or more events in the stream. Only the fields indicated in **selectExpression** are modified. The events to be modified are indicated by the index value of each member of the **items** collection. The individual events in **items** also hold the new events.

If there is a problem patching any individual event, the entire operation is rolled back and the error will indicate the streamId and index of the problem.  

**.NET Library**
```csharp
    Task PatchValueAsync(string streamId, string selectExpression, T item);
    Task PatchValuesAsync(string streamId, string selectExpression, IList<T> items);
```

**********************

## ``Remove Values``

There are two options for specifying which events to remove from a stream:
* [Index Collection](#removeindexcollection): One or more indexes can be specified in the request. 
* [Window](#removewindow): A window can be specified with a start index and end index.

<a name="removeindexcollection"></a>
### `Index Collection`

Removes the event at each index from the specified stream. Different overloads are available to make it easier to indicate the index where you want to remove a data event. 

**Request**  

        DELETE api/v1-preview/Tenants/{tenantId}/Namespaces/{namespaceId}/Streams/{streamId}/Data
		    ?index={index}[&index={index} …]

**Parameters**  
``string tenantId``  
The tenant identifier  
  
``string namespaceId``  
The namespace identifier   
  
``string streamId``  
The stream identifier  
  
``string index``  
One or more indexes of events to remove

**Response**  
The response includes a status code

**_Notes_**  
If any individual event fails to be removed, the entire operation is rolled back and no events are removed. The streamId and index that caused the issue are included in the error response. 

If you attempt to remove events at indexes that have no events, an error is returned. If this occurs, you can use [Window](#removewindow) request format to remove any events from a specified ‘window’ of indexes, which will not return an error if no data is found.

**.NET Library**
```csharp
    Task RemoveValueAsync(string streamId, string index);
    Task RemoveValueAsync<T1>(string streamId, T1 index);
    Task RemoveValueAsync<T1, T2>(string streamId, Tuple<T1, T2> index);

    Task RemoveValuesAsync(string streamId, IEnumerable<string> index);
    Task RemoveValuesAsync<T1>(string streamId, IEnumerable<T1> index);
    Task RemoveValuesAsync<T1, T2>(string streamId, IEnumerable<Tuple<T1, T2>> 
```

<a name="removewindow"></a>

### `Window`

Removes events at and between the start index and end index.

**Request**

        DELETE api/v1-preview/Tenants/{tenantId}/Namespaces/{namespaceId}/Streams/{streamId}/Data
		    ?startIndex={startIndex}&endIndex={endIndex}

**Parameters**  
``string tenantId``  
The tenant identifier  
  
``string namespaceId``  
The namespace identifier   
  
``string streamId``  
The stream identifier  
  
``string startIndex``  
The index defining the beginning of the window

``string endIndex``  
The index defining the end of the window  

**Response**  
The response includes a status code

**_Notes_**  
If any individual event fails to be removed, the entire operation is rolled back and no removes are done.

**.NET Library**
```csharp
    Task RemoveWindowValuesAsync(string streamId, string startIndex, string endIndex);
    Task RemoveWindowValuesAsync<T1>(string streamId, T1 startIndex, T1 endIndex);
    Task RemoveWindowValuesAsync<T1, T2>(string streamId, Tuple<T1, T2> startIndex, Tuple<T1, T2> endIndex);
```

***********************  

## ``Replace Values``

Writes one or more events over existing events in the specified stream.

**Request**

        PUT api/v1-preview/Tenants/{tenantId}/Namespaces/{namespaceId}/Streams/{streamId}/Data
		    ?allowCreate=false

**Parameters**  
``string tenantId``  
The tenant identifier  
  
``string namespaceId``  
The namespace identifier   
  
``string streamId``  
The stream identifier  
 
**Request Body**
A serialized list of one or more events of the stream type

**Response**  
The response includes a status code

**_Notes_**  
This request returns an error if the stream does not have an event to be replaced at the specified index. If any individual event fails to be replaced, the entire operation is rolled back and no replaces are performed. The index that caused the issue and the streamId are included in the error response.

**.NET Library**
```csharp
    Task ReplaceValueAsync<T>(string streamId, T item);
    Task ReplaceValuesAsync<T>(string streamId, IList<T> items);
```

***********************

## `Update Values`

Writes one or more events to the specified stream.

**Request**

        PUT api/v1-preview/Tenants/{tenantId}/Namespaces/{namespaceId}/Streams/{streamId}/Data

**Parameters**  
``string tenantId``  
The tenant identifier  
  
``string namespaceId``  
The namespace identifier   
  
``string streamId``  
The stream identifier  

**Request Body**  
A serialized list of one or more events of the stream type

**Response**  
The response includes a status code

**_Notes_**  
This request performs an insert or a replace depending on whether an event already exists at the event indexes. If any item fails to write, the entire operation is rolled back and
no events are written to the stream. The index that caused the issue is included in the error response.

**.NET Library**
```csharp
    Task UpdateValueAsync<T>(string streamId, T item);
    Task UpdateValuesAsync<T>(string streamId, IList<T> items);
```

***********************

## `Bulk Insert Values`

Writes specified events to multiple streams.

**Request**

        POST api/Tenants/{tenantId}/Namespaces/{namespaceId}/Bulk/Streams/Data

**Parameters**  
``string tenantId``  
The tenant identifier
  
``string namespaceId``  
The namespace identifier

**Request Body**  
A serialized list of streams and lists of events

**Response**  
An array of stuff MOTODO: Description

*****

## `Bulk Insert Values`

Updates specified events for multiple streams.

**Request**  

        PUT api/Tenants/{tenantId}/Namespaces/{namespaceId}/Bulk/Streams/Data

**Parameters**  
``string tenantId``  
The tenant identifier
  
``string namespaceId``  
The namespace identifier

**Request Body**  
A serialized list of streams and lists of events

**Response**  
An array of stuff MOTODO: Description
