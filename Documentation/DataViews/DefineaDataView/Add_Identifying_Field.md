---
uid: AddIdentifyingField
---

# Add identifying field
If a field set resolves to multiple data items in any group (or if grouping is not used), then one field should be designated as the field set's `.IdentifyingField`. If one lone criterion is not a sufficient or useful way of disambiguating the fields, then [grouping](xref:DataViewsGrouping) by additional criteria may be necessary.  Any field from field sources `FieldSource.Id`, `FieldSource.Name`, `FieldSource.Metadata`, `FieldSource.Tags` can be used as an identifying field. Keys are required for identifying fields with the source type of `FieldSource.Metadata` and `FieldSource.Tags`. Keys are not applicable for identifying fields with the source type of `FieldSource.Id` and `FieldSource.Name`.

## Example: Add an identifying field
Let us take a subset of the [example scenario](xref:DataViewsExampleScenario) power inverter streams, returned by the `Query` [value](xref:sdsSearching) `"TypeId:docs-pi-inverter AND Site:Winterthur"`. 

| Site | Meter | Measurement | Stream Id | Tags |
|--|--|--|--|--|
| Winterthur | Primary | Power In | WINT.Meter.Primary.Inverter.0.PwrIn | Low Resolution |
| Winterthur | Primary | Power Out | WINT.Meter.Primary.Inverter.0.PwrOut | Low Resolution |
| Winterthur | Secondary | Power In | WINT.Meter.Secondary.Inverter.0.PwrIn | Low Resolution |
| Winterthur | Secondary | Power Out | WINT.Meter.Secondary.Inverter.0.PwrOut | Low Resolution |

The following represents a data view grouped by "Meter", including fields for the grouping value, and each data item's "Tags" and property "Value":

```json
{
  "Id": "quickstart",
  "Queries": [
    {
      "Id": "inverters",
      "Value": "TypeId:docs-pi-inverter AND Site:Winterthur"
    }
  ],
  "DataFieldSets": [
    {
      "QueryId": "inverters",
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
  ],
   "GroupingFields": [
    {
      "Source": "Metadata",
      "Keys": [ "Meter" ],
      "Label": "{IdentifyingValue} {FirstKey}"
    }
  ]
}
```

The view resolves into two groups of field mappings:

| Timestamp.0 | Meter.1 | Value.2 | Tags.3 | Value.4 | Tags.5 | Value.6 | Tags.7 | Value.8 | Tags.9 |
|--|--|--|--|--|--|--|--|--|--|
| - | Primary | ..Primary..PwrIn/PropertyId:Value | ..Primary..PwrIn/Tags |  ..Primary..PwrOut/PropertyId:Value | ..Primary..PwrOut/Tags  | | | | |
| - | Secondary |  |  |  |  | ..Secondary..PwrIn/PropertyId:Value | ..Secondary..PwrIn/Tags |  ..Secondary..PwrOut/PropertyId:Value | ..Secondary..PwrOut/Tags  |

Two things are clearly undesirable here:
1. The field identifiers are ambiguous
2. The result is sparse: the data views engine has not been told how to align the data items across groups, so it has no idea that all "Power In" streams are similar.

To remedy this situation, add an `.IdentifyingField` to the field set.

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
