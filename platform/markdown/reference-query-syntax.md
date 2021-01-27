# Overview

Generally queries will consist of a `where` clause plus optional [modifiers](#query-modifiers) controlling the specific subset of results returned. 

# Where Clause

The Where clause must be a non-empty array containing not more than 10 conditions. For some operators, `value` will be an array. See the following general syntax example:
[block:code]
{
  "codes": [
    {
      "code": "{\n  where: [\n    [<fieldName>, <operator>, <value>],\n    [<fieldName>, <array operator>, [<value1>, <value2>]] \n  ] \n}",
      "language": "json",
      "name": "Syntax"
    }
  ]
}
[/block]
## Fields
Valid fields consist of the indices defined for the document being queried. For example, the [DPNS data contract](https://github.com/dashevo/dpns-contract/blob/master/schema/dpns-contract-documents.json) defines three indices:

| Index Field(s) | Index Type | Unique |
| - | - | :-: | 
| [normalizedParentDomainName, normalizedLabel](https://github.com/dashevo/dpns-contract/blob/master/schema/dpns-contract-documents.json#L4-L14) | Compound | Yes |
| [records.dashUniqueIdentityId](https://github.com/dashevo/dpns-contract/blob/master/schema/dpns-contract-documents.json#L15-L22) | Single Field | Yes |
| [records.dashAliasIdentityId](https://github.com/dashevo/dpns-contract/blob/master/schema/dpns-contract-documents.json#L23-L29) | Single Field | No |


[block:html]
{
  "html": "<div></div>\n<!--\nSpecial fields - `$id`, `$userId`\n-->\n<style></style>"
}
[/block]
## Comparison Operators

| Name | Description |
| :-: | - |
| < | Matches values that are less than a specified value |
| <= | Matches values that are less than or equal to a specified value |
| == | Matches values that are equal to a specified value |
| => | Matches values that are greater than or equal to a specified value |
| > | Matches values that are greater than a specified value |
| in | Matches all document(s) where the value of the field equals any value in the specified array <br>Array may include up to 100 (unique) elements |

## Array Operators

| Name | Description |
| :-: | - |
| length | Selects documents if the array field is a specified size (integer) |
| contains | - Matches arrays that contain all elements specified in the query condition array <br>- 100 element maximum
| elementMatch |  - Matches documents that contain an array field with at least one element that matches all the criteria in the query condition array <br>- Two or more conditions must be provided

## Evaluation Operators

| Name | Description |
| :-: | - |
| startsWith | Selects documents where the value of a field begins with the specified characters (string, <= 255 characters) |

## Operator Examples
[block:code]
{
  "codes": [
    {
      "code": "{\n  where: [\n    ['nameHash', '<', '56116861626961756e6176657a382e64617368'],\n  ],\n}",
      "language": "json",
      "name": "<"
    },
    {
      "code": "{\n  where: [\n      ['normalizedParentDomainName', '==', 'dash'],\n      // Return all matching names from the provided array\n      ['normalizedLabel', 'in', ['alice', 'bob']],\n    ]\n}",
      "language": "json",
      "name": "in"
    },
    {
      "code": "{\n  where: [\n      ['normalizedParentDomainName', '==', 'dash'],\n      // Return any names beginning with \"al\" (e.g. alice, alfred)\n      ['normalizedLabel', 'startsWith', 'al'],\n    ]\n}",
      "language": "json",
      "name": "startsWith"
    },
    {
      "code": "{\n  where: [\n      // Return documents that have 5 values in their `items` array\n      ['items', 'length', 5],\n    ]\n}",
      "language": "json",
      "name": "length"
    },
    {
      "code": "{\n  where: [\n      // Return documents that have both \"red\" and \"blue\" \n      // in the `colors` array\n      ['colors', 'contains', ['red', 'blue']],\n    ]\n}",
      "language": "json",
      "name": "contains"
    },
    {
      "code": "{\n  where: [\n    // Return `scores` documents where the results contain \n    // elements in the range 80-90\n    ['scores', 'elementMatch',\n      [\n        ['results', '>=', '80'],\n        ['results', '<=', '90']\n      ],\n    ],\n  ]\n}",
      "language": "json",
      "name": "elementMatch"
    }
  ]
}
[/block]
# Query Modifiers
The query modifiers described here determine how query results will be sorted and what subset of data matching the query will be returned.

| Modifier | Effect | Example |
| - | - | - |
| `limit` | Restricts the number of results returned (maximum: 100) | `limit: 10` |
| `orderBy` | Returns records sorted by the field(s) provided (maximum: 2) | `orderBy: [['normalizedLabel', 'asc']]`
| `startAt` | Returns records beginning with the index provided (minimum: 1, maximum: 20000) | `startAt: 100` |
| `startAfter` | Returns records beginning after the index provided  (maximum: 20000) | `startAfter: 50` |
[block:callout]
{
  "type": "warning",
  "title": "Compound Index Constraints",
  "body": "For indices composed of multiple fields ([example from the DPNS data contract](https://github.com/dashevo/dpns-contract/blob/master/schema/dpns-contract-documents.json)), the sort order in an `orderBy` must either match the order defined in the data contract OR be the inverse order. Please see the [mongoDB documention](https://docs.mongodb.com/manual/core/index-compound/#sort-order) for further details related to this topic.\nAdditionally, the order in which the properties are defined in a compound index affects how queries may be constructed per this [mongoDB documentation](https://docs.mongodb.com/manual/core/index-compound/#prefixes). For example, a DPNS query for `normalizedLabel` must also include `normalizedParentDomainName` while the inverse is not true ([index definition](https://github.com/dashevo/dpns-contract/blob/master/schema/dpns-contract-documents.json) in the DPNS contract)."
}
[/block]

[block:callout]
{
  "type": "danger",
  "body": "Currently inverse sort functionality for compound indices is not available on Evonet. [Drive PR #313](https://github.com/dashevo/drive/pull/313) will enable this functionality once deployed.",
  "title": "Compound Index Inverse Sort"
}
[/block]
# Example query
The following query combines both a where clause and query modifiers.
[block:code]
{
  "codes": [
    {
      "code": "{\n    where: [\n        ['normalizedParentDomainName', '==', 'dash'],\n        ['normalizedLabel', 'startsWith', 'test'],\n    ],\n    startAt: 15,\n    limit: 20,\n    orderBy: [\n        ['records.dashUniqueIdentityId', 'asc']\n    ]\n}",
      "language": "json"
    }
  ]
}
[/block]
# MongoDB Comparision

Currently Dash Platform uses mongoDB for storage. The following table shows a mapping of platform operators to the underlying mongoDB operators that are used internally:

| Platform Operator | mongoDB Operator | 
| :-: | :-: | 
| < | [$lt](https://docs.mongodb.com/manual/reference/operator/query/lt) |
| <= | [$lte](https://docs.mongodb.com/manual/reference/operator/query/lte) |
| == | [$eq](https://docs.mongodb.com/manual/reference/operator/query/eq) |
| => | [$gte](https://docs.mongodb.com/manual/reference/operator/query/gte) |
| > | [$gt](https://docs.mongodb.com/manual/reference/operator/query/gt) |
| in | [$in](https://docs.mongodb.com/manual/reference/operator/query/in) |
| length | [$size](https://docs.mongodb.com/manual/reference/operator/query/size/) |
| startsWith | [$regex](https://docs.mongodb.com/manual/reference/operator/query/regex/) |
| elementMatch | [$elemMatch](https://docs.mongodb.com/manual/reference/operator/query/elemMatch) |
| contains | [$all](https://docs.mongodb.com/manual/reference/operator/query/all) |