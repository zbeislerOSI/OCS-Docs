---
uid: AddIdentifyingField
---

# Add identifying field
If a field set resolves to multiple data items in any group (or if grouping is not used), then one field should be designated as the field set's `.IdentifyingField`. If one lone criterion is not a sufficient or useful way of disambiguating the fields, then [grouping](xref:DataViewsGrouping) by additional criteria may be necessary.  Any field from field sources `FieldSource.Id`, `FieldSource.Name`, `FieldSource.Metadata`, `FieldSource.Tags` can be used as an identifying field. Keys are required for identifying fields with the source type of `FieldSource.Metadata` and `FieldSource.Tags`. Keys are not applicable for identifying fields with the source type of `FieldSource.Id` and `FieldSource.Name`.

