# Platform Error Codes

A comprehensive set of consensus error codes were introduced  in Dash Platform v0.21. The tables below follow the codes found in [code.js](https://github.com/dashevo/platform/blob/master/packages/js-dpp/lib/errors/consensus/codes.js) of the consensus source code.

The error codes are organized into four categories that each span 1000 error codes. Each category may be further divided into sub-categories. The four categories and their error code ranges are:

| Category | Code range | Description |
| - | :-: | - |
| [Basic](#basic) | 1000 - 1999 | Errors encountered while validating structure and data |
| [Signature](#signature) | 2000 - 2999 | Errors encountered while validating identity existence and state transition signature |
| [Fee](#fee) | 3000 - 3999 | Errors encountered while validating an identity's balance is sufficient to pay fees |
| [State](#state) | 4000 - 4999 | Errors encounter while validating state transitions against the platform state |

# Basic

Basic errors occupy the codes ranging from 1000 to 1999. This range is divided into several categories for clarity.

## Decoding

| Code | Error | 
| :-: | - | 
| 1000 | ProtocolVersionParsingError |  |
| 1001 | SerializedObjectParsingError |  |

## General

| Code | Error | 
| :-: | - | 
| 1002 | UnsupportedProtocolVersionError |  |
| 1003 | IncompatibleProtocolVersionError |  |
| 1004 | JsonSchemaCompilationError |  |
| 1005 | JsonSchemaError |  |
| 1006 | InvalidIdentifierError |  |

## Data Contract

| Code | Error | |
| :-: | - | - |
| 1007 | DataContractMaxDepthExceedError |  |
| 1008 | DuplicateIndexError |  |
| 1009 | IncompatibleRe2PatternError |  |
| 1010 | InvalidCompoundIndexError |  |
| 1011 | InvalidDataContractIdError |  |
| 1012 | InvalidIndexedPropertyConstraintError |  |
| 1013 | InvalidIndexPropertyTypeError |  |
| 1014 | InvalidJsonSchemaRefError |  |
| 1015 | SystemPropertyIndexAlreadyPresentError |  |
| 1016 | UndefinedIndexPropertyError |  |
| 1017 | UniqueIndicesLimitReachedError |  |
| 1048 | DuplicateIndexNameError | Added in v0.22 |
| 1050 | InvalidDataContractVersionError | **4013 prior to v0.23** |
| 1051 | IncompatibleDataContractSchemaError | **4014 prior to  v0.23** |
| 1052 | DataContractImmutablePropertiesUpdateError | **4015 prior to v0.23** |
| 1053 | DataContractIndicesChangedError | **4016 prior to v0.23** |
| 1054 | DataContractInvalidIndexDefinitionUpdateError | **Added in v0.23** |
| 1055 | DataContractHaveNewUniqueIndexError | **Added in v0.23** |

## Document

| Code | Error | 
| :-: | - | 
| 1018 | DataContractNotPresentError |  |
| 1019 | DuplicateDocumentTransitionsWithIdsError |  |
| 1020 | DuplicateDocumentTransitionsWithIndicesError |  |
| 1021 | InconsistentCompoundIndexDataError |  |
| 1022 | InvalidDocumentTransitionActionError |  |
| 1023 | InvalidDocumentTransitionIdError |  |
| 1024 | InvalidDocumentTypeError |  |
| 1025 | MissingDataContractIdError |  |
| 1026 | MissingDocumentTransitionActionError |  |
| 1027 | MissingDocumentTransitionTypeError |  |
| 1028 | MissingDocumentTypeError |  |

## Identity

| Code | Error | |
| :-: | - | - |
| 1029 | DuplicatedIdentityPublicKeyError |  |
| 1030 | DuplicatedIdentityPublicKeyIdError |  |
| 1031 | IdentityAssetLockProofLockedTransactionMismatchError |  |
| 1032 | IdentityAssetLockTransactionIsNotFoundError |  |
| 1033 | IdentityAssetLockTransactionOutPointAlreadyExistsError |  |
| 1034 | IdentityAssetLockTransactionOutputNotFoundError |  |
| 1035 | InvalidAssetLockProofCoreChainHeightError |  |
| 1036 | InvalidAssetLockProofTransactionHeightError |  |
| 1037 | InvalidAssetLockTransactionOutputReturnSizeError |  |
| 1038 | InvalidIdentityAssetLockTransactionError |  |
| 1039 | InvalidIdentityAssetLockTransactionOutputError |  |
| 1040 | InvalidIdentityPublicKeyDataError |  |
| 1041 | InvalidInstantAssetLockProofError |  |
| 1042 | InvalidInstantAssetLockProofSignatureError |  |
| 1046 | MissingMasterPublicKeyError | Added in v0.22 |
| 1047 | InvalidIdentityPublicKeySecurityLevelError | Added in v0.22 |
| 1056 | InvalidIdentityKeySignatureError | **Added in v0.23** |

## State Transition

| Code | Error | 
| :-: | - | 
| 1043 | InvalidStateTransitionTypeError |  |
| 1044 | MissingStateTransitionTypeError |  |
| 1045 | StateTransitionMaxSizeExceededError |  |

# Signature

Signature errors occupy the codes ranging from 2000 to 2999.

| Code | Error |  |
| :-: | - | - |
| 2000 | IdentityNotFoundError |  |
| 2001 | InvalidIdentityPublicKeyTypeError |  |
| 2002 | InvalidStateTransitionSignatureError |  |
| 2003 | MissingPublicKeyError |  |
| 2004 | InvalidSignaturePublicKeySecurityLevelError | **Added in v0.23** |
| 2005 | WrongPublicKeyPurposeError | **Added in v0.23** |
| 2006 | PublicKeyIsDisabledError | **Added in v0.23** |
| 2007 | PublicKeySecurityLevelNotMetError | **Added in v0.23** |

# Fee

Fee errors occupy the codes ranging from 3000 to 3999.

| Code | Error | |
| :-: | - | - |
| 3000 | BalanceIsNotEnoughError | Current credits balance is insufficient to pay fee |

# State

State errors occupy the codes ranging from 4000 to 4999. This range is divided into several categories for clarity.

## Data Contract

| Code | Error | |
| :-: | - | - |
| 4000 | DataContractAlreadyPresentError |  |
| 4001 | DataTriggerConditionError |  |
| 4002 | DataTriggerExecutionError |  |
| 4003 | DataTriggerInvalidResultError |  |
| ~~4013~~ | *InvalidDataContractVersionError* | ⚠️ **Renumbered to [1050](#data-contract) in v0.23** |
| ~~4014~~ | *IncompatibleDataContractSchemaError* | ⚠️ **Renumbered to [1051](#data-contract) in v0.23** |
| ~~4015~~ | *DataContractImmutablePropertiesUpdateError* | ⚠️ **Renumbered to [1052](#data-contract) in v0.23** |
| ~~4016~~ | *DataContractIndicesChangedError* | ⚠️ **Renumbered to [1053](#data-contract) in v0.23** |

## Document

| Code | Error | 
| :-: | - | 
| 4004 | DocumentAlreadyPresentError |  |
| 4005 | DocumentNotFoundError |  |
| 4006 | DocumentOwnerIdMismatchError |  |
| 4007 | DocumentTimestampsMismatchError |  |
| 4008 | DocumentTimestampWindowViolationError |  |
| 4009 | DuplicateUniqueIndexError |  |
| 4010 | InvalidDocumentRevisionError |  |

## Identity

| Code | Error | |
| :-: | - | - |
| 4011 | IdentityAlreadyExistsError |  |
| 4012 | IdentityPublicKeyDisabledAtWindowViolationError | **Added in v0.23** |
| 4017 | IdentityPublicKeyIsReadOnlyError | **Added in v0.23** |
| 4018 | InvalidIdentityPublicKeyIdError | **Added in v0.23** |
| 4019 | InvalidIdentityRevisionError | **Added in v0.23** |
| 4020 | StateMaxIdentityPublicKeyLimitReachedError | **Added in v0.23** |
| 4021 | DuplicatedIdentityPublicKeyStateError | **Added in v0.23** |
| 4022 | DuplicatedIdentityPublicKeyIdStateError | **Added in v0.23** |
| 4023 | IdentityPublicKeyIsDisabledError | **Added in v0.23** |