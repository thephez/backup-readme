# Misc

Maximum size of CBOR-encoded data: 16 KB (https://github.com/dashevo/js-dpp/pull/114)
Maximum size of serialized data contract: 15 KB (https://github.com/dashevo/js-dpp/pull/117) 


# Data Contract Documents

Many of the constraint details come from [js-dpp/schema/meta/data-contracts.json](https://github.com/dashevo/js-dpp/blob/v0.11-dev/schema/meta/data-contract.json).


Misc Restrictions 
- Can't use these keywords: `default`
- Can't use remote `$ref` (i.e. `http://<something>`)
- `additionalProperties` must always be `false` ???
- Can't use `propertyNames` (https://github.com/dashevo/js-dpp/pull/107)
- Maximum data contract depth: 500

## Properties

List of property definitions

### Requirements

### Constraints

| Description | Value |
| - | - |
| Minimum number of properties | 1 |
| Maximum number of properties | 100 |
| Minimum property name length | 1 |
| Maximum property name length | 63 |
| Property name first/last characters | ** Alphanumeric only (`A-Z`, `a-z`, `0-9`)**|
| Property name characters | Alphanumeric (`A-Z`, `a-z`, `0-9`)<br>Hypen (`-`) <br>Underscore (`_`) |

## Indices

Array of properties to use in index creation

### Requirements

- List of included document properties
- Unique / not-unique (duplicate property values allowed)

### Constraints

| Description | Value |
| - | - |
| Maximum number of indices | 10 |
| Maximum number of unique indices | 3 |
| Maximum number of properties in a single index | 10 |

## Required Properties

List of document properties that must be included with every document of this type that is submitted to the platform.

Example from the DPNS `preorder` document:
```json
    "required": [
      "saltedDomainHash"
    ],
```

## Additional Properties

Additional properties are not allowed. The data contract documents object must explicitly indicate this by including the following:
```json
"additionalProperties": false
```