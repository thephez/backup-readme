# Overview
At any given point in time, the data stored by each application (and more broadly, the entire platform) is in a specific state. State transitions are the means for submitting data that creates, updates, or deletes platform data and results in a change to a new state.

For example, Alice may have already added Bob and Carol as friends in DashPay while also having a pending friend request to Dan. If Dan declines the friend request, the state will transition to a new one where Alice and Bob remain in Alice’s friend list while Dan moves to the declined list.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/74f641a-State_Transition_example_4.png",
        "State Transition example (4).png",
        644,
        258,
        "#d7e1e9"
      ],
      "sizing": "smart",
      "border": false,
      "caption": "State Transition Example"
    }
  ]
}
[/block]

# Implementation Overview

To ensure the consistency and integrity of data stored on Layer 2, all data is governed by the [Dash Platform Protocol](explanation-platform-protocol) (DPP) which describes serialization and validation rules. Since state transitions are the vehicle for delivering data to the platform, the implementation of state transitions resides in DPP alongside the validation logic. 

## Structure

To support the various data types used on the platform and enable future updates, state transitions were designed to be flexible. Each state transition consists of a:
1. Header - version and payload type
2. Payload - contents vary depending on payload type
3. Signature - signature of the header/payload by the identity submitting to state transition

The following table contains a list of currently defined payload types:

| Payload Type | Payload Description |
| - | - |
| Data Contract (`0`) | Database schema for a single application |
| Documents Batch (`1`) | An array of 1 or more document transition objects containing application data |
| Identity (`2`) | Identity creation information |

## Application Usage

State transitions are constructed by client-side libraries and then submitted to the platform via DAPI. Based on the validation rules described in DPP (and an application data contract where relevant), Dash Platform first validates the state transition. Some state transitions (e.g. data contracts, identity) are validated solely by explicit rules defined in DPP, while others (e.g. documents) are also subject to the rules defined by the relevant application’s data contract. Once the state transition has been validated, the platform stores the data and updates the platform state.