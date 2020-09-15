---
uid: ocsPropertiesTable
---
# Properties table

To accompany API template

## Table description

| Property Name | Data Type | Required | Default Value | Searchable | Description |
|-------|------|----------|---------|------------|---------|
|    Name of the property   | Data type of the property     |    Is this property optional or required?      | Default value if the property value is not set        |   Is the property searchable?         |   Description of the property      |
|       |      |   This is an optional column       |   This is an optional column      |      This is an optional column      |         |

## Example properties tables

| Property Name | Data Type |  Required | Default Value | Searchable | Description |
|-------|------|----------|---------|------------|---------|
|  Id     | String     |     Yes     |        |     Yes       |    Unique identifier     |
|  Tag     | String[]     |         |   []       |            |    Gets or sets for OSIsoft internal use only     |
|  GroupingFields     | Field[]     | No        |  []       |            |   Fields by which the data items are partitioned/grouped     |

| Property Name | Data Type |  Required | Default Value | Searchable | Description |
|-------|------|----------|---------|------------|---------|
|  Interpolation mode    | SdsInterpolationMode     |    No     |  Continuous (0)      |    No       |    Interpolation setting of the type |