# Overview

Generally queries will consist of a `where` clause plus optional [modifiers](#query-modifiers) controlling the specific subset of results returned. 
[block:callout]
{
  "type": "warning",
  "title": "Query limitations",
  "body": "Dash Platform v0.22 introduced a number of limitations due to the switch to using [GroveDB](https://github.com/dashevo/grovedb). See details in pull requests [77](https://github.com/dashevo/platform/pull/77) and [230](https://github.com/dashevo/platform/pull/230) that implemented these changes.\n\nQuery validation details may be found [here](https://github.com/dashevo/platform/blob/master/packages/js-drive/lib/document/query/validateQueryFactory.js) along with the associated validation [tests](https://github.com/dashevo/platform/blob/master/packages/js-drive/test/unit/document/query/validateQueryFactory.spec.js)."
}
[/block]
# Where Clause

The Where clause must be a non-empty array containing not more than 10 conditions. For some operators, `value` will be an array. See the following general syntax example:
[block:callout]
{
  "type": "danger",
  "body": "As of Dash Platform v0.22, _all fields_ referenced in a query's where clause must be defined in the _same index_. This includes `$createdAt` and `$updatedAt`."
}
[/block]

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

Valid fields consist of the indices defined for the document being queried. For example, the [DPNS data contract](https://github.com/dashevo/platform/blob/master/packages/dpns-contract/schema/dpns-contract-documents.json) defines three indices:

| Index Field(s) | Index Type | Unique |
| - | - | :-: | 
| [normalizedParentDomainName, normalizedLabel](https://github.com/dashevo/platform/blob/master/packages/dpns-contract/schema/dpns-contract-documents.json#L5-L16) | Compound | Yes |
| [records.dashUniqueIdentityId](https://github.com/dashevo/platform/blob/master/packages/dpns-contract/schema/dpns-contract-documents.json#L17-L25) | Single Field | Yes |
| [records.dashAliasIdentityId](https://github.com/dashevo/platform/blob/master/packages/dpns-contract/schema/dpns-contract-documents.json#L26-L33) | Single Field | No |
[block:html]
{
  "html": "<div></div>\n<!--\nSpecial fields - `$id`, `$userId`\n-->\n<style></style>"
}
[/block]
## Comparison Operators

### Equal

| Name | Description |
| :-: | - |
| == | Matches values that are equal to a specified value |

### Range
[block:callout]
{
  "type": "warning",
  "title": "Dash Platform v0.22 notes",
  "body": "- Only one range operator is allowed in a query (except for between behavior)\n- The `in` operator is only allowed for last two indexed properties\n- Range operators are only allowed after `==` and `in` operators\n- Range operators are only allowed for the last two fields used in the where condition\n- Queries using range operators must also include an `orderBy` statement"
}
[/block]
| Name | Description |
| :-: | - |
| < | Matches values that are less than a specified value |
| <= | Matches values that are less than or equal to a specified value |
| >= | Matches values that are greater than or equal to a specified value |
| > | Matches values that are greater than a specified value |
| in | Matches all document(s) where the value of the field equals any value in the specified array <br>Array may include up to 100 (unique) elements |

## Array Operators
[block:callout]
{
  "type": "danger",
  "title": "NOTE",
  "body": "The following operators are not available in Dash Platform v0.22."
}
[/block]
| Name | Description |
| :-: | - |
| length | Selects documents if the array field is a specified size (integer) |
| contains | - Matches arrays that contain all elements specified in the query condition array <br>- 100 element maximum
| elementMatch | - Matches documents that contain an array field with at least one element that matches all the criteria in the query condition array <br>- Two or more conditions must be provided

## Evaluation Operators

| Name | Description |
| :-: | - |
| startsWith | Selects documents where the value of a field begins with the specified characters (string, <= 255 characters). Must include an `orderBy` statement. |

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
      "code": "// Not available in Dash Platform v0.22\n// See https://github.com/dashevo/platform/pull/77\n{\n  where: [\n      // Return documents that have 5 values in their `items` array\n      ['items', 'length', 5],\n    ]\n}",
      "language": "json",
      "name": "length"
    },
    {
      "code": "// Not available in Dash Platform v0.22\n// See https://github.com/dashevo/platform/pull/77\n{\n  where: [\n      // Return documents that have both \"red\" and \"blue\" \n      // in the `colors` array\n      ['colors', 'contains', ['red', 'blue']],\n    ]\n}",
      "language": "json",
      "name": "contains"
    },
    {
      "code": "// Not available in Dash Platform v0.22\n// See https://github.com/dashevo/platform/pull/77\n{\n  where: [\n    // Return `scores` documents where the results contain \n    // elements in the range 80-90\n    ['scores', 'elementMatch',\n      [\n        ['results', '>=', '80'],\n        ['results', '<=', '90']\n      ],\n    ],\n  ]\n}",
      "language": "json",
      "name": "elementMatch"
    }
  ]
}
[/block]
# Query Modifiers
The query modifiers described here determine how query results will be sorted and what subset of data matching the query will be returned.
[block:callout]
{
  "type": "danger",
  "title": "Breaking changes",
  "body": "Starting with Dash Platform v0.22, `startAt` and `startAfter` must be provided with a document ID rather than an integer."
}
[/block]
| Modifier | Effect | Example |
| - | - | - |
| `limit` | Restricts the number of results returned (maximum: 100) | `limit: 10` |
| `orderBy` | Returns records sorted by the field(s) provided (maximum: 2). Sorting must be by the last indexed property. Can only be used with `>`, `<`, `>=`, `<=`, and `startsWith` queries. | `orderBy: [['normalizedLabel', 'asc']]`
| `startAt` | Returns records beginning with the document ID provided | `startAt: Buffer.from(Identifier.from(<document ID>))` |
| `startAfter` | Returns records beginning after the document ID provided | `startAfter: Buffer.from(Identifier.from(<document ID>))` |
[block:callout]
{
  "type": "warning",
  "title": "Compound Index Constraints",
  "body": "For indices composed of multiple fields ([example from the DPNS data contract](https://github.com/dashevo/platform/blob/master/packages/dpns-contract/schema/dpns-contract-documents.json)), the sort order in an `orderBy` must either match the order defined in the data contract OR be the inverse order.\n\nPlease refer to [pull request 230](https://github.com/dashevo/platform/pull/230) for additional information related to compound index constraints in Platform v0.22."
}
[/block]
# Example query
The following query combines both a where clause and query modifiers.
[block:code]
{
  "codes": [
    {
      "code": "import Dash from \"dash\"\n\nconst { Essentials: { Buffer }, PlatformProtocol: { Identifier } } = Dash;\n\nconst query = {\n  limit: 5,\n  startAt: Buffer.from(Identifier.from('4Qp3menV9QjE92hc3BzkUCusAmHLxh1AU6gsVsPF4L2q')),\n  where: [\n    ['normalizedParentDomainName', '==', 'dash'],\n    ['normalizedLabel', 'startsWith', 'test'],\n  ],\n  orderBy: [\n    ['normalizedLabel', 'asc'],\n  ],\n}",
      "language": "javascript"
    }
  ]
}
[/block]