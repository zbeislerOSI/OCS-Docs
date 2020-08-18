---
uid: ocsPropertiesTable
---
# Properties table
To be used to accompany API

## Table description
| Property Name | Data Type | Required | Default | Searchable | Description |
|-------|------|----------|---------|------------|---------|
|    name of the property   | data type of the property     |   Is this property optional or required?       |  default value or format       |   Is the property searchable?         |   description of the property      |
|       |      |   This is an optional column       |   This is an optional column      |      This is an optional column      |         |


## Example properties table
| Property Name | Data Type | Required | Default | Searchable | Description |
|-------|------|----------|---------|------------|---------|
|  Id     | String     | Yes         |         |     Yes       |    Unique identifier     |
|  GroupingFields     | Field[]     | No        |  []       |     Yes       |   Fields by which the data items are partitioned/grouped     |

## Questions for writers
Questions for writers:
1. Do you prefer calling it a `field name` or `property name`?
2. What about `details` vs `description`?
3. Please leave your comments in the conversation channel. 
