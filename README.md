
# stx-AuthentiChain - Intellectual Property Protection Smart Contract (Clarity)

This smart contract is designed to securely **register, verify, transfer, and manage** intellectual property (IP) records on the **Stacks blockchain**. It ensures cryptographic integrity, ownership control, and optional expiration for IP assets.

---

## 📌 Features

* ✅ **Register new IP** with a unique SHA-256 hash and optional expiration block
* 🔍 **Verify** ownership and hash integrity of registered IP
* 🔄 **Transfer** IP ownership between principals
* ✏️ **Update metadata** (hash) of an IP record
* ⏳ **Extend expiration** of an IP record
* 🔐 Enforced **access control** and strong **error handling**

---

## 🛠️ Smart Contract Overview

### 📂 Data Structures

#### Maps

* `ip-registrations`: Stores IP records (`ip-id`, `owner`, `timestamp`, `hash`, `expiration`)
* `registered-hashes`: Tracks registered hashes and their associated `ip-id`

#### Data Variables

* `ip-counter`: Auto-incremented ID for each IP
* `owner`: Deployer of the contract

---

### 🚀 Public Functions

#### `register-ip (ip-hash (buff 32)) (expiration-block (optional uint))`

Registers a new intellectual property by hash.

* Validates hash length, non-zero, and uniqueness
* Sets optional expiration
* Returns new `ip-id`

#### `transfer-ip (ip-id uint) (new-owner principal)`

Transfers ownership of the IP to a new principal.

* Only the current owner can transfer

#### `extend-ip-registration (ip-id uint) (new-expiration uint)`

Extends the expiration date for a registered IP.

* Only allowed by current owner
* Must be a future block height

#### `update-ip-metadata (ip-id uint) (new-hash (buff 32))`

Updates the metadata (hash) for an IP record.

* Must be owned by caller and not expired
* Removes old hash and sets the new one

---

### 🔎 Read-only Functions

#### `check-ip-ownership (ip-id uint)`

Returns the owner of a given `ip-id` if it exists.

#### `verify-ip-hash (ip-id uint) (hash-to-verify (buff 32))`

Verifies whether a given hash matches the one registered for a specific `ip-id`.

#### `is-hash-registered (ip-hash (buff 32))`

Checks if a hash is already registered.

#### `verify-ip-owner (ip-id uint)`

Returns the owner and expiration date of an IP record, or error if expired.

---

### ⚠️ Error Codes

| Error Constant                | Code    | Description                            |
| ----------------------------- | ------- | -------------------------------------- |
| `ERR-NOT-AUTHORIZED`          | `u1000` | Caller is not the owner                |
| `ERR-INVALID-HASH-LENGTH`     | `u1001` | IP hash is not 32 bytes                |
| `ERR-HASH-ALL-ZEROS`          | `u1002` | IP hash is all zeros                   |
| `ERR-HASH-ALREADY-REGISTERED` | `u1003` | This hash has already been registered  |
| `ERR-IP-NOT-FOUND`            | `u1004` | No record found for provided `ip-id`   |
| `ERR-INVALID-IP-ID`           | `u1005` | `ip-id` is invalid (zero or malformed) |
| `ERR-IP-ID-OUT-OF-RANGE`      | `u1006` | `ip-id` exceeds current registered max |
| `ERR-IP-EXPIRED`              | `u1007` | IP record has expired                  |
| `ERR-INVALID-EXPIRATION`      | `u1008` | Expiration is invalid or in the past   |
| `ERR-NO-EXPIRATION-SET`       | `u1009` | No expiration exists on record         |

---

## 📈 Use Cases

* Proof-of-ownership for digital assets (art, documents, inventions)
* Decentralized intellectual property ledger
* Blockchain-based copyright registry
* Integration with NFT metadata integrity
* Patent or trademark registration on-chain

---

## ✅ Deployment

You can deploy this smart contract using:

* **Clarity REPL**
* **Stacks.js**
* **Clarity tools (e.g., Clarinet)**

Ensure you're using the latest version of the Stacks blockchain and Clarity VM.

---

## 🔐 Security Considerations

* All inputs are strictly validated
* Ownership checks prevent unauthorized actions
* Hash uniqueness enforced to prevent duplicate claims
* Time-sensitive actions are validated with block height

---

## 🧪 Example Workflow

1. **Register** a digital work:

   ```clarity
   (register-ip 0xABC123... (some u3000))
   ```
2. **Verify** hash integrity:

   ```clarity
   (verify-ip-hash u1 0xABC123...)
   ```
3. **Transfer** ownership:

   ```clarity
   (transfer-ip u1 'ST123...')
   ```
4. **Extend** registration:

   ```clarity
   (extend-ip-registration u1 u5000)
   ```
5. **Update** IP metadata:

   ```clarity
   (update-ip-metadata u1 0xDEF456...)
   ```

---

## 🏷️ License

MIT License – Use freely for public and commercial purposes. Contributions welcome!

---