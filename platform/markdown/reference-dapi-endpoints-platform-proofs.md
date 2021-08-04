Since data verification is a critical aspect of Dash Platform, all [Platform endpoints](reference-dapi-endpoints-platform-endpoints) can provide an optional proof that the response is correct. Set the optional `prove` parameter (`"prove": true`) in the request to receive a proof that contains the requested data.

# Proof Structure

Each proof consists of four parts:

| Field | Type | Description |
|-|-|-|
| rootTreeProof | Bytes (base64) | Merkle path to the `storeTreeProof` |
| [storeTreeProof](#store-tree-proof) | Bytes (base64) | Merk tree proof containing the store root hash, the merkle path, and the requested data |
| signatureLlmqHash | Bytes (base64) | Hash of the LLMQ that created the `signature` |
| signature | Bytes (base64) | Signature of the merkle root of the `rootTreeProof` |
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"proof\": {\n    \"rootTreeProof\": \"AQAAAANDyOH1e+nnMfZAiT9W6O8bnSwNt7ExdoPORAjOOQl1hncTjwAKTjkBt9itDASm0uhlmZTIAE7qaorXXpW9W4TMJu/rY51+fQEeMotzZc6ndsHCaWC3rvrAWkzYni1iBYgBBQ==\",\n    \"storeTreeProof\": \"Aa3xeZMhgUlpOmObZgsBc/A7Tw8DiE7Al0d8UnlPKDxEAkxb4mgZiSCI/Dww08L/Kypsf9zOSuQ9pbuCf1+iLKo2EAFJN2x17KAW3m6kfE4Vbv2sI5P9cRgajTYzLrYc41t5VRE=\",\n    \"signatureLlmqHash\": \"AAABDRGoCpOki/CVlNkMeQhapUFyIoXOESFPn1cXK6I=\",\n    \"signature\": \"AU+FZUCH/OKrM8GuNo0L8S+mKCpbAe/f9SJ1V3fhudrlOja1KislQRcBbIShk74bGLvN8exT/6BVxPHQtaM+LDqfuZGXG1EmgNGY2827/Pyn8XGtP2jDmuhvonCRD4QN\"\n  },\n  \"metadata\": {\n    \"height\": 433,\n    \"coreChainLockedHeight\": 9801\n  }\n}\n",
      "language": "json",
      "name": "Example Response with Proof"
    }
  ]
}
[/block]
## Root tree proof
[block:callout]
{
  "type": "info",
  "body": "Details regarding the root tree proofs and their verification will be provided in a future update to this page."
}
[/block]
## Store tree proof

Store tree proofs are based on a modified version of [Merk](https://github.com/nomic-io/merk/). Some details from the Merk documentation are included below. Additional details are available in the [Algorithms document](https://github.com/nomic-io/merk/blob/develop/docs/algorithms.md) on the Merk repository.

### Structure 

Merk proofs are a list of stack-based operators and node data, with 3 possible operators: `Push(node)`, `Parent`, and `Child`. A stream of these operators can be processed by a verifier in order to reconstruct a sparse representation of part of the tree, in a way where the data can be verified against a known root hash.

The value of `node` in a `Push` operation can be one of three types:

* `Hash(hash)` - The hash of a node
* `KVHash(hash)` - The key/value hash of a node
* `KV(key, value)` - The key and value of a node

### Binary Format

We can efficiently encode these proofs by encoding each operator as follows:

| Operator | Op. Value | Size | Description |
|-|:-:|-|-|
| Push(Hash(hash)) | `0x01` | 32 bytes | Node hash |
| Push(KVHash(hash)) | `0x02` | 32 bytes | Node key/value hash |
| Push(KV(key, value)) | `0x03` | < 1-byte key length ><br>< n-byte key ><br>< 2-byte value length ><br>< n-byte value > | Node key/value |

This results in a compact binary representation, with a very small space overhead (roughly 2 bytes per node in the proof (1 byte for the Push operator type flag, and 1 byte for a Parent or Child operator), plus 3 bytes per key/value pair (1 byte for the key length, and 2 bytes for the value length)).

# Retrieving response data from proofs

The function below shows a simple example of parsing a response's `storeTreeProof` to retrieve the data asked for by the request:
[block:code]
{
  "codes": [
    {
      "code": "// Get data from base64 encoded store tree proof\nfunction getStoreProofData(storeProof) {\n  const buf = Buffer.from(storeProof, 'base64');\n\n  let x = 0;\n  let valueFound = false;\n  while (x < buf.length) {\n    const type = buf.readUInt8(x);\n    x += 1;\n\n    switch (type) {\n      case 0x01: { // Hash\n        x += hashLength;\n        break;\n      }\n\n      case 0x02: { // Key/value hash\n        x += hashLength;\n        break;\n      }\n\n      case 0x03: { // Key / Value\n        const keySize = buf.readUInt8(x);\n        x += 1;\n        x += keySize;\n\n        const valueSize = buf.readUInt16BE(x);\n        x += 2;\n\n        // Value\n        const value = buf.toString('hex', x, x + valueSize);\n        x += valueSize;\n        const map = cbor.decode(value);\n\n        valueFound = true;\n        return map;\n      }\n\n      case 0x10: // Parent\n        break;\n\n      case 0x11: // Child\n        break;\n\n      default:\n        console.log(`Unknown type: ${type.toString(16)}`);\n        break;\n    }\n  }\n  console.log(`Value found: ${valueFound}`);\n}",
      "language": "javascript"
    }
  ]
}
[/block]