---
icon: pickaxe
---

# Rainbow Bitwork Whitepaper

## Overview

Rainbow Bitwork is a system used to define the difficulty of verifying transactions on the blockchain. It calculates the verification difficulty based on blockchain height and transaction fees, dynamically adjusting to changes in the network. Rainbow Bitwork provides a flexible difficulty calculation mechanism to ensure the security and efficiency of the blockchain. This document outlines the structure and functioning of Rainbow Bitwork, including specific examples.

## Bitwork Structure

In Rainbow Bitwork, the difficulty is represented by a simple two-part structure:

* **`pre`**: A number representing the primary difficulty level, with a range of 0 to 64.
* **`post_hex`**: A single hexadecimal character that provides additional difficulty precision.

The `Bitwork` structure provides a compact representation of difficulty values and includes strict validation rules. It can be represented as a numerical structure or converted into a string format for ease of use in the system.

## Validation Rules

Rainbow Bitwork applies strict validation rules to ensure that `Bitwork` values are valid. The main rules are:

* The `pre` value must be between 0 and 64.
* The `post_hex` must be a valid single hexadecimal character.
* If `pre` equals 64, then `post_hex` must be "0".

Any `Bitwork` instance that violates these rules is considered invalid.

### Example

Valid `Bitwork` example:

* `pre = 8`, `post_hex = 0`, string representation: `8.0`.

Invalid `Bitwork` examples:

* `pre = 65` (invalid because `pre` exceeds 64).
* `pre = 8`, `post_hex = fa` (invalid because `post_hex` has multiple characters).

## Bitwork Calculation

Rainbow Bitwork allows calculating `Bitwork` values based on blockchain height and transaction fees. The system defines two main calculation methods.

### Use Case: Block-height Bitwork Calculation

Rainbow Bitwork calculates the difficulty based on the difference between the current blockchain height and the protocol deployment height. The `pre` and `post_hex` values are computed using the following formulas:

$$
\text{pre} = \left\lfloor \frac{\text{current\_height} - \text{deploy\_height}}{\text{difficulty\_epoch} \times 16} \right\rfloor
$$

$$
\text{post\_hex} = \left(\frac{\text{current\_height} - \text{deploy\_height}}{\text{difficulty\_epoch}} \mod 16\right)
$$

#### Example

If the current height is 850,000, the deployment height is 840,000, and the difficulty epoch is 256, the calculation results are:

* `pre = 2`
* `post_hex = 1`
* String representation: `2.1`

This example shows that as the blockchain height increases, the difficulty gradually increases, but it will not reach the maximum value of `64.0` with such a small difference in height.

In a previous example, the difficulty at block height 850,000 was incorrectly described as `64.0`. In reality, the difficulty only approaches or reaches 64 when the difference between `current_height` and `deploy_height` is large enough. For the example above, with a deployment height of 840,000, the difficulty is `2.1`.

### Use Case: Fee-Based Bitwork Calculation

Rainbow Bitwork also supports calculating difficulty based on transaction fees. By taking the natural logarithm of the fee, the `pre` value is extracted from the integer part, and `post_hex` is derived from the fractional part. The formulas are:

$$
\text{pre} = \left\lfloor \ln(\text{fee}) \right\rfloor
$$

$$
\text{post\_hex} = \left(\ln(\text{fee}) - \left\lfloor \ln(\text{fee}) \right\rfloor\right) \times 16
$$

#### Example

For a transaction fee of 160,000 satoshis, the calculation results are:

* `pre = 11`
* `post_hex = f`
* String representation: `11.f`

## Bitwork Merging

Rainbow Bitwork allows merging two `Bitwork` instances, often used to combine height-based and fee-based difficulties. The process involves adding the two `pre` values and adjusting `post_hex`. If the resulting `pre` exceeds 64, it is capped at 64.

### Merging Rules:

* If the merged `pre` exceeds 64, the `pre` value is set to 64.
* The merged `post_hex` is recalculated to account for overflow.

### Example

**Merging two Bitwork instances:**

Suppose we have two `Bitwork` instances:

* `pre_1 = 3`, `post_hex_1 = 9`
* `pre_2 = 4`, `post_hex_2 = 8`

According to the merging rules:

1. Add the `pre` values: `pre_1 + pre_2 = 3 + 4 = 7`
2. Add the `post_hex` values in hexadecimal: `9 + 8 = 17`, where `17` in hexadecimal is `11`.
3. Handle the carry: Add the carry of `1` to `pre`, resulting in `pre = 7 + 1 = 8`, and set `post_hex = 1`.

The final merged `Bitwork` is:

* `pre = 8`
* `post_hex = 1`
* String representation: `8.1`

Another example:

* `pre = 8`, `post_hex = 0`
* `pre = 9`, `post_hex = 0`

The merged result is:

* `pre = 17`
* `post_hex = 0`
* String representation: `17.0`

## Bitwork and Hash Matching

Rainbow Bitwork provides a mechanism for verifying whether a hash meets a specific difficulty.

### Matching Algorithm:

1. **Define the target hash**: Target hashe should be 32-bytes hash, we can easily use existing blockhash or use random one, we will give example of existing hash (we reverse it later) in the following section.
2. **Check the first `pre` characters**: Ensure the first `pre` characters of the reversed hash match the target.
3. **Compare the `pre + 1` character**: The `pre + 1` character of the reversed hash must be greater than or equal to `post_hex`.

#### Example

Suppose the original transaction hash is:

```
4a4bb0de3f382355947ff711474d06dd5932ca108ec0cadbb794a04196ddbce8
```

We want to find a hash that meets `Bitwork = 10.d`.

**Step 1: Reverse the transaction hash**

Reversing the hash gives:

```
e8bcdd9641a097b4dbca0c8e10ca3259dd064d4711f77f945523383fde0b4b4a
```

**Step 2: Match the first `pre = 10` characters**

The first 10 characters of the reversed hash are:

```
e8bcdd9641
```

These characters meet the condition, so no adjustment is needed.

**Step 3: Compare the `pre + 1` character**

The 11th character is `a`, but `Bitwork = 10.d` requires that the 11th character be at least `d`.

**Step 4: Adjust the hash**

To meet the `Bitwork = 10.d` requirement, we change the 11th character to `d`:

```
e8bcdd9641**d**097b4dbca0c8e10ca3259dd064d4711f77f945523383fde0b4b4a
```

This new hash satisfies the difficulty condition of `Bitwork = 10.d`.

## Practical Use Cases

1. **Simple Epoch-Based Difficulty Adjustment** Difficulty adjusts at regular intervals (epochs). After each epoch, if blocks were mined faster than expected, the difficulty increases by a small increment (e.g., one decimal place). If slower, the difficulty decreases similarly. This ensures steady block production over time.
2. **Height-Based Difficulty Adjustment**\
   The difficulty dynamically adjusts based on the current blockchain height. As the blockchain grows, the difficulty increases. However, the increase is based on the relative difference between the protocol deployment block and the current block height, not fixed thresholds. For example, when the deployment height is 840,000 and the current height is 850,000, the Bitwork is `2.1`, far from the maximum `64.0`.
3. **Fee-Based Difficulty Adjustment**\
   Higher transaction fees result in higher verification difficulty. This ensures that larger transactions are harder to verify, preventing spam and rewarding validators for processing high-value transactions. For instance, a very large transaction fee near 10^19 results in a `Bitwork` of `43.b`.

## Adding more rule engines to the protocol

By incorporating additional rule engines, Rainbow Bitwork can further strengthen its security and prevent tampering. These engines add extra layers of validation, making it harder for attackers to manipulate the system. More rules mean better adaptability to changing network conditions and stronger protection against attacks.

Here's the example:

### Example: Proof of Work Submission with Bitwork Difficulty `3.a`

Suppose we have a target hash with a Bitwork difficulty of `3.a`. How can we ensure that the proof of work submitted by the user is both valid and authentic?

1. **Reverse the Target Hash**:\
   First, reverse the transaction hash from big-endian to little-endian. Let's call it `reversedId`.
2. **Taproot Magic**:\
   Next, we construct pseudo-transactions following these rules:
   * **Commit Transaction**:
     * **Input#0**: (txid `reversedId`, vin: 0, value: **MagicValueConst**)
     * **Output#0**: (address: Bitcoin Lockscript computed P2TR address, value: any)
   *   **Bitcoin Lockscript**:

       ```html
       <pubkey-hash>
       OP_CHECKSIG     // Verifies the signature against the pubkey-hash
       OP_FALSE
       OP_IF
       0x0462746370    // Protocol Bytes, "rbo" for example
       <Mine>          // Followed by a single push to denote the operation type
       <Payload>       // Payload (CBOR encoded) for the operation
       OP_ENDIF
       ```

       The **Payload** can be in JSON format:

       ```json
       {"t": "DMT", "nonce": 0, "time": 1}
       ```
   * **Reveal Transaction**:
     * **Input#0**: References Output#0 of the Commit Transaction.
     * **Output#0**: The **Sender's Address**.
   * We take the **Target Hash** and **Bitwork (3.a)** and compare them with the **Commit Transaction ID**.
   * Both transactions (Commit and Reveal) must be signed by the **Sender's Address** and submitted to the validator to verify the proof of work.
3. **Ensuring Authenticity**:\
   The structure above ensures the following:
   1. The data structure follows specific rule engines with protocol fields embedded in the payload. In this case:
      * There are two transactions (Commit and Reveal), and the sender acts as both the signer of the commit and the reveal.
      * The protocol fields are signed, making it easy for the validator to verify.
   2. The Taproot transactions are signed by the sender, and only they can unlock the reveal transaction. The validator can **verify the signature** and transaction without needing to broadcast it on the Bitcoin network.
   3. The Commit Transaction ID is used to verify the Bitwork difficulty. The proof of work process involves **iterating over transaction parameters** until the Bitwork target (3.a) is met.

## Conclusion

Rainbow Bitwork provides a flexible and powerful mechanism for adjusting blockchain verification difficulty. By calculating difficulty based on blockchain height and transaction fees, the system dynamically adapts to network conditions, ensuring fairness and security in validation. Rainbow Bitwork’s strict validation rules and efficient merging mechanism make it an integral part of blockchain networks.

***
