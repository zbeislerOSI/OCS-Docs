---
uid: AddIdentifyingField
---

# Identifying field
If the field set resolves to multiple data items in any group (or if grouping is not used), then a field should be designated as the field set's `.IdentifyingField`. If one lone criterion is not a sufficient or useful way of disambiguating the fields, then [grouping](xref:DataViewsGrouping) by additional criteria may be necessary.

## Example: Adding an identifying field
To the data view from the previous example, we will add a `Field` as the `.IdentifyingField` of its field set. In this example, it makes sense to identify each data item by its _Measurement_.

```json
{
  "Id": "quickstart",
  "Queries": [
    {
      "Id": "inverters",
      "Value": "TypeId:docs-pi-inverter AND Site:Winterthur"
    }
  ],
  "GroupingFields": [
    {
      "Source": "Metadata",
      "Keys": [ "Meter" ],
      "Label": "{IdentifyingValue} {FirstKey}"
    }
  ],
  "DataFieldSets": [
    {
      "QueryId": "inverters",
      "IdentifyingField": {
          "Source": "Metadata",
          "Keys": [ "Measurement" ]
      },
      "DataFields": [
        {
          "Source": "PropertyId",
          "Keys": [ "Value" ],
          "Label": "{IdentifyingValue} {FirstKey}"
        },
        {
          "Source": "Tags",
          "Keys": [ "Low Resolution", "High Resolution" ],
          "Label": "{IdentifyingValue} Tags"
        }
      ]
    }
  ]
}
```

The result is much more consumable. The data field identifiers are no longer ambiguous, and like data items are aligned across groups:

| Timestamp | Meter | Power In Value | Power In Tags | Power Out Value | Power Out Tags |
|--|--|--|--|--|--|
| - | Primary | ..Primary..PwrIn/PropertyId:Value | ..Primary..PwrIn/Tags |  ..Primary..PwrOut/PropertyId:Value | ..Primary..PwrOut/Tags  |
| - | Secondary | ..Secondary..PwrIn/PropertyId:Value | ..Secondary..PwrIn/Tags |  ..Secondary..PwrOut/PropertyId:Value | ..Secondary..PwrOut/Tags  |
