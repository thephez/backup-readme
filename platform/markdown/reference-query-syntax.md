Generally queries will consist of a `where` clause plus optional modifiers controlling the specific subset of results returned. 

| Modifier | Effect | Example |
| - | - | - |
| `limit` | Restricts the number of results returned (maximum: 100) | `limit: 10` |
| `orderBy` | Returns records sorted by the field(s) provided (maximum: 2) | `orderBy: [['normalizedLabel', 'asc']]`
| `startAt` | Returns records beginning with the index provided (maximum: 20000) | `startAt: 100` |
| `startAfter` | Returns records beginning after the index provided  (maximum: 20000) | `startAfter: 50` |
[block:callout]
{
  "type": "warning",
  "title": "Compound Index Constraints",
  "body": "For indices composed of multiple fields ([example from the DPNS data contract](https://github.com/dashevo/dpns-contract/blob/master/src/schema/dpns-documents.json#L11-L14)), the sort order in an `orderBy` must either match the order defined in the data contract OR be the inverse order. Please see the [mongoDB documention](https://docs.mongodb.com/manual/core/index-compound/#sort-order) for further details related to this topic."
}
[/block]

[block:callout]
{
  "type": "danger",
  "body": "Currently inverse sort functionality for compound indices is not available on Evonet. [Drive PR #313](https://github.com/dashevo/drive/pull/313) will enable this functionality once deployed.",
  "title": "Compound Index Inverse Sort"
}
[/block]
# Where Clause Details

The Where clause must be a non-empty array containing not more than 10 conditions. General syntax: `{ where: [ [<fieldName>, <operator>, <condition>], [<fieldName>, <operator>, <condition>] ] }`

## Fields
Valid fields consist of the indices defined for the document being queried. For example, the [DPNS data contract](https://github.com/dashevo/dpns-contract/blob/master/src/schema/dpns-documents.json) defines three indices:

| Index Field(s) | Index Type | Unique |
| - | - | :-: | 
| [nameHash](https://github.com/dashevo/dpns-contract/blob/master/src/schema/dpns-documents.json#L4-L9) | Single Field | Yes |
| [normalizedParentDomain, normalizedLabel](https://github.com/dashevo/dpns-contract/blob/master/src/schema/dpns-documents.json#L10-L15) | Compound | No |
| [records.dashIdentity](https://github.com/dashevo/dpns-contract/blob/master/src/schema/dpns-documents.json#L16-L20) | Single Field | Yes |


[block:html]
{
  "html": "<div></div>\n<!--\nSpecial fields - `$id`, `$userId`\n-->\n<style></style>"
}
[/block]
## Operators
- `<`, `<=`, `==`, `>`, `>=`
- `in` (array, 100 element maximum, unique elements)
- `startsWith` (string, <= 255 characters)
- `elementMatch` (requires 2 conditions)
- `length`  (0 or positive integer)
- `contains` (scalar or array of scalars only, 100 element maximum, unique elements)
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
    }
  ]
}
[/block]
# Example query
The following query combines both a where clause and query modifiers.
[block:code]
{
  "codes": [
    {
      "code": "{\n    where: [\n        ['normalizedParentDomainName', '==', 'dash'],\n        ['normalizedLabel', 'startsWith', 'test'],\n    ],\n    startAt: 15,\n    limit: 20,\n    orderBy: [\n        ['nameHash', 'asc']\n    ]\n}",
      "language": "json"
    }
  ]
}
[/block]