(Explanation from Andy Freer, a nice starting point)

Identities represent a user on the blockchain, not a payment address.  DashDrive lets users store documents (in other words data Entities), the syntax and validity of which is defined in a Contract, and the ownership of which is proved by the sig of the identity that created them.

Now, if Alice and Bob have identities and want to pay each other by name, we can construct a simple pay-by-name DAP as such:

1. 3rd party dev (Carol) creates a Contract that allows identities to store a list of other identities (this will be each user's Contact list),
2. Alice wants to pay Bob so first she has to create an Entity with Bob's identity using Carol's Contract, and embeds an HDPubKey, encrypted for Bob. (we can call this a Contact Entity)
3. Bob then sees the entity with his identity included (using a client that's reading Carol's Contract) and interprets this as a Contact request.
4. Bob decides to accept the request, so he creates his own Entity using Carol's Contract, which contains Alice's identity and Bob's HDPubKey, encrypted for her.
5. Alice then sees that Bob created an entity for her using Carol's Contract, and her client interprets this as an accepted request (because both users have essentially exchanged payment address seeds).
6. Alice and Bob can now pay each other by name, using a client working with Carol's Contract, which can generate a unique payment address  behind-the-scenes each time they pay by name.
7. Alice and Bob's clients can scan the blockchain using the exchanged addresses to generate a history of all payments between themselves

This is the basics of the DashPay DAP thats coming (its probably improved by now but this is the essence of it)...the main benefit is that the Contact entities Alice and Bob created for each other are stored 'on the Dash blockchain' so they never need to trust a third party that the past transactions between those names, or when making a new payment they dont need to worry if they are paying the right person, they can have full confidence and never see an address.  They can also create / verify / access this data from a stateless client (like a browser) without a 3rd party and the data is secured and available 24/7 on the Masternode network, and with the payment addresses and transactions private between the users.

Also note that each Contact entity is in itself a unique digital asset owned by Alice and Bob..  Alice and Bob control ownership, content and transfer of their entities, Carol controls the format and rules for what those entities can contain and what state they can be in.  For more advanced DAPs this is a really neat feature.