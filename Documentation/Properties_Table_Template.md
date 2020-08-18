---
uid: ocsPropertiesTable
---
# Properties table
To accompany API template

## Table description
| Property Name | Data Type | Default Type | Required | Searchable | Description |
|-------|------|----------|---------|------------|---------|
|    name of the property   | data type of the property     |   default value or formatIs      |   this property optional or required?       |   Is the property searchable?         |   description of the property      |
|       |      |   This is an optional column       |   This is an optional column      |      This is an optional column      |         |


## Example properties table
| Property Name | Data Type |  Default Type | Required | Searchable | Description |
|-------|------|----------|---------|------------|---------|
|  Id     | String     |          |   Yes      |     Yes       |    Unique identifier     |
|  Tag     | String[]     | Array        |          |            |    Gets or sets for OSIsoft internal use only     |
|  GroupingFields     | Field[]     | Array        |  No       |            |   Fields by which the data items are partitioned/grouped     |

## Questions for writers
Questions for writers:
1. Do you prefer calling it a `field name` or `property name`?  F:1 P: 2 E: 1
2. What about `details` vs `description`? Desc: all
3. Please leave your comments in the conversation channel. 
