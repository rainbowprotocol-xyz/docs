# ✨ RBO Tokens

## Definition and Issuance&#x20;

In the traditional financial system, asset issuance is typically managed by central institutions that control the creation and distribution of assets. However, the advent of blockchain technology has made decentralized asset issuance possible. On the Bitcoin network, asset issuance can be achieved by creating tokens that represent various types of assets, from digital currencies to digital representations of physical assets.

Rainbow Protocol makes asset issuance on the Bitcoin network easier and more efficient by providing standardized and simplified asset issuance scripts. Rainbow Protocol writes data envelopes in Taproot's segregated witness data, including protocol feature codes and operation fields, with other data encapsulated in CBOR format. The data format is as follows:

```
<pubkey-hash>
OP_CHECKSIG     // Perform a check signature against the pubkey-hash
OP_FALSE
OP_IF
 0x0462746370 // Push "rbo" 3 bytes
 <Operation>  // Followed by a single push to denote the operation type
 <Payload>    // Payload (CBOR encoded) for the operation
OP_ENDIF
```

These assets are defined as RBO Tokens in Rainbow Protocol. We currently support the opcode "Init," and only scripts that comply with the rules will create RBO assets.

## RBO Tokens

Rainbow Protocol defines fungible tokens as RBO Tokens. These assets are issued on the Bitcoin network, following Bitcoin Taproot transaction standards:

* A Commit transaction calculates the unlocking address and participates in the asset issuance declaration.
* A Reveal transaction signs the unlocking address and establishes issuance rights.
* After the Reveal transaction is confirmed, minting and transfer transactions can only be initiated after six blocks.

**Issuance Script**:

```
<pubkey-hash>
OP_CHECKSIG     // Perform a check signature against the pubkey-hash
OP_FALSE
OP_IF
 0x0462746370 // Push "btcp" 4 bytes
 <init>  // Followed by a single push to denote the operation type
 <init-payload>    // Payload (CBOR encoded) for the operation
OP_ENDIF

```



**Token Definition（**init-payload**）**:

```
{
    "met": {
        "d": "1a2b3d4c5e...", // Token icon
        "ct": "image/svg+xml", // Icon mime-type
        "term": "This is an experimental Fungible Token" // Token description
    },
    "t": "FT", // Default type is fungible token
    "n": "RBO", // Token name
    "sym": "ℝ", // Token symbol
    "sup": "21000000", // Total supply
    "amt": "10000", // Single mint amount
    "dec": "0", // Decimal places
    "fee": "0", // Fee rate
    "pre": "0", // Pre-mint amount (currently not open)
    "to": "bc1p...", // Mint to a specific address (currently not open)
    "ath": 888888, // Activation height, must be greater than reveal + 6
}

```
