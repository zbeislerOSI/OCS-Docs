---
uid: ConsolidateDataFields
---

# Consolidate data fields

There is a problem with this data view: the Gen1 Weather streams have a "Temperature" property but the Gen2 Weather streams have  renamed it to "AmbientTemperature". The data view has fields for both, but for each stream "Temperature" or "AmbientTemperature" is null. This is undesirable since the fields are semantically identical and should not be separate fields in the data view.

Find the `Field` associated with "AmbientTemperature". To the `Field`'s `.Keys` array, add "Temperature":

```json
{
  "Source": "PropertyId",
  "Keys": [
    "AmbientTemperature",
    "Temperature"
  ],
  "Label": "{IdentifyingValue} {FirstKey}"
},
```
Now that field will match to either "Temperature" or "AmbientTemperature".  
Remove the `Field` associated only with "Temperature".

You can also remove the `Fields` associated with data item Id and Name. These are not providing any additional useful information.

```json
PUT /api/v1/Tenants/{tenantId}/Namespaces/{namespaceId}/DataViews/quickstart
{
  "Id": "quickstart",
  "Name": "quickstart",
  "IndexField": { "Label": "Timestamp" },
  "Queries": [
    { 
      "Id": "weather",
      "Value":"*weather*" 
    }
  ],
  "DataFieldSets": [
        {
            "QueryId": "weather",
            "DataFields": [
                {
                    "Source": "PropertyId",
                    "Keys": [
                        "AmbientTemperature",
                        "Temperature"
                    ],
                    "Label": "{IdentifyingValue} {FirstKey}"
                },
                {
                    "Source": "PropertyId",
                    "Keys": [
                        "CloudCover"
                    ],
                    "Label": "{IdentifyingValue} {FirstKey}"
                },
                {
                    "Source": "PropertyId",
                    "Keys": [
                        "SolarRadiation"
                    ],
                    "Label": "{IdentifyingValue} {FirstKey}"
                },
                {
                    "Source": "Tags",
                    "Keys": [
                        "Weather",
                        "Low Resolution",
                        "High Resolution",
                        "Gen1",
                        "Gen2",
                    ],
                    "Label": "{IdentifyingValue} Tags"
                }
            ],
            "IdentifyingField": {
                "Source": "Metadata",
                "Keys": [
                    "Site"
                ],
                "Label": "{IdentifyingValue} {FirstKey}"
            }
       },
  ],
  "GroupingFields": [],
  "IndexTypeCode": "DateTime",
  "Shape": "Standard"
}
```
## Expected result
```text
HTTP 204 No Content
```

## Action
```text
GET /api/v1/Tenants/{tenantId}/Namespaces/{namespaceId}/DataViews/quickstart/Data/Interpolated
?startIndex={your_val_here}&endIndex={your_val_here}&interval={your_val_here}
```

## Expected result
```json
HTTP 200 OK
[
    {
        "Timestamp": "2019-10-21T18:00:00Z",
        "Biltmore AmbientTemperature": 33.589619124193831,
        "Biltmore CloudCover": null,
        "Biltmore SolarRadiation": 165,
        "Biltmore Tags": "Weather, High Resolution, Gen1",
        "Rosecliff AmbientTemperature": 14.537369185607899,
        "Rosecliff CloudCover": null,
        "Rosecliff SolarRadiation": 132,
        "Rosecliff Tags": "Weather, Low Resolution, Gen1",
        "Winterthur AmbientTemperature": 1.0805517883941373,
        "Winterthur CloudCover": 2,
        "Winterthur SolarRadiation": 108,
        "Winterthur Tags": "Weather, High Resolution, Gen2"
    }
    ...
]
```

