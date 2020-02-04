# Overview

Although [data contracts](explanation-platform-protocol-data-contract) provide much needed constraints on the structure of the data being stored on Dash Platform, there are limits to what they can do. Certain system data contracts may require server-side logic to operate effectively. For example, [DPNS](doc:explanation-dpns) must enforce some rules to ensure names remain DNS compatible. [Dash Platform Protocol](explanation-platform-protocol) (DPP) supports this application-specific custom logic using Data Triggers.
[block:callout]
{
  "type": "danger",
  "title": "Constraints",
  "body": "Given a number of technical considerations (security, masternode processing capacity, etc.), data triggers are not considered a platform feature at this time. They are currently hard-coded in Dash Platform Protocol and not available for use in non-system data contracts."
}
[/block]
# Details
Since all application data is submitted in the form of documents, data triggers are defined in the context of documents. To provide even more granularity, they also incorporate the document `action` so separate triggers can be created for the `CREATE`, `REPLACE`, or `DELETE` actions.

As an example, DPP contains several [data triggers for DPNS](https://github.com/dashevo/js-dpp/tree/master/lib/dataTrigger/dpnsTriggers). The `preorder` document has no extra constraints, but the `domain` document requires additional validation:

| Data Contract | Document | Action(s) | Trigger Description |
| - | - | - |
| DPNS | `domain` | [`CREATE`](https://github.com/dashevo/js-dpp/blob/master/lib/dataTrigger/dpnsTriggers/createDomainDataTrigger.js) | Enforces DNS compatibility and validates provided hashes |
| DPNS | `domain` | [`REPLACE`](https://github.com/dashevo/js-dpp/blob/master/lib/dataTrigger/dpnsTriggers/updateDomainDataTrigger.js) | Prevents updates to existing domains |
| DPNS | `domain` | [`DELETE`](https://github.com/dashevo/js-dpp/blob/master/lib/dataTrigger/dpnsTriggers/deleteDomainDataTrigger.js) | Prevents deletion of existing domains |
| ---- | ----| ---- | ---- |
| DPNS | `preorder` | `CREATE`, `REPLACE`, `DELETE` | No triggers defined for preorders |

When document state transitions are received, DPP checks if there is a trigger associated with the document type and action. If there is, it then executes the trigger logic. Successful execution of the trigger logic is necessary for the document to be accepted and applied to the [platform state](explanation-drive-platform-state).