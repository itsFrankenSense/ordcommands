# OrdCommands

This is a personal database for keeping track of various commands and their examples.

## Quick References:
- [Dry Run Testing](#dry-run-(testing))
- [1. Normal Inscription](#1-normal-inscription)
- [2. Normal Inscription and send to specified address](#2-normal-inscription-and-send-to-specified-address) 
- [3. Reduced Padding Inscription](#3-reduced-padding-inscription)
- [4. Inscription with metadata](#4-inscription-with-metadata)
- [5. Provenance Inscription](#5-provenance-inscription)
- [6. Batch Inscription](#6-batch-inscription)
- [7. Inscribe on a specific sat (must have 2 UTXOs in wallet)](#7-inscribe-on-a-specific-sat-must-have-2-utxos-in-wallet)
- [8. Start indexing with Satoshi tracking](#8-start-indexing-with-Satoshi-tracking)

---

### Dry Run (testing)   
`--dry-run`Dry run allows you to construct the transaction without signing or broadcasting. This provides a fee estimate and confirms the rest of your command is constructed correctly. `--dry-run` can be added to the end of any commands.

Command: `ord wallet inscribe --fee-rate <FEE_RATE> --file "FILE" --dry-run`  
Example: `ord wallet inscribe --fee-rate 10 "D:\Files\1.txt" --dry-run`

---

### 1 Normal Inscription
A standard inscription.

Command: `ord wallet inscribe --fee-rate <FEE_RATE> --file "FILE"`  
Example: `ord wallet inscribe --fee-rate 10 "D:\Files\1.txt"`

---
### 2 Normal Inscription and send to specified address  
`--destination <ADDRESS>` Inscribe and send to specified address.

Command: `ord wallet inscribe --fee-rate <FEE_RATE> --file "FILE" --destination <ADDRESS>`   
Example: `ord wallet inscribe --fee-rate 10 --postage 330sats --file "D:\Files\1.txt" --destination bc1pwdreq8nxulu96p2zrxmmfgcnhfg05vlw85clqx22s5qtx9ah9dzshcj4lc`

---
### 3 Reduced Padding Inscription
[Example Inscription padded with 330 sats](https://ordinals.com/inscription/a86a426fe273f330238765cd941477fa3f647dc9235cf36ba4c3e8b56064c335i4)
`--postage <POSTAGE>`Inscribe with a specified postage for reduced padding (default is 10k sats).

Command: `ord wallet inscribe --fee-rate <FEE_RATE> --postage <POSTAGE> --file "FILE"`   
Example: `ord wallet inscribe --fee-rate 10 --postage 330sats --file "D:\Files\1.txt"`

---

### 4 Inscription with metadata
[Example Inscription](https://ordinals.com/inscription/cadc6c906fcf340452c7ad40ce59dafb207b685026a18606531534f121d6c301i0)  
`--json-metadata <JSON_METADATA>`Inscribe with JSON metadata.

Command: `ord wallet inscribe --fee-rate <FEE_RATE> --json-metadata <JSON_METADATA> --file "FILE"`  
Example: `ord wallet inscribe --fee-rate 10 --json-metadata "D:\Files\metadata.json" --file "D:\Files\1.txt"`

<details>
  <summary> Example Metadata structure </summary>
  
```json
{
  "title": "Unique Digital Artwork",
  "artist": "Creative Artist",
  "description": "A unique piece of digital art created by Creative Artist.",
  "year": 2023,
  "type": "Digital Art",
  "tags": ["abstract", "colorful", "modern"],
  "limited_edition": true,
  "copy_number": 1,
  "total_copies": 100
}
```
</details>

---

### 5 Provenance Inscription
[Example Parent Inscription](https://ordinals.com/inscription/b03f8f87e64eeab788ad62c3ffe35e9a875c97ca802ec0ae04d932584acdc475i0)  
`--parent <PARENT_INSCRIPTION_ID>` Inscribe a file with a specified parent inscription ID.

Command: `ord wallet inscribe --fee-rate <FEE_RATE> --parent <PARENT_INSCRIPTION_ID> --file "CHILD_FILE"`  
Example: `ord wallet inscribe --fee-rate 10 --parent b03f8f87e64eeab788ad62c3ffe35e9a875c97ca802ec0ae04d932584acdc475i0 --file "D:\Files\1.txt"`

---

### 6 Batch Inscription
[Example Batch Inscription](https://ordinals.com/inscription/cadc6c906fcf340452c7ad40ce59dafb207b685026a18606531534f121d6c301i0)  
`--batch "BATCH_FILE"` Perform batch inscriptions by specifying a batch file.

Command: `ord wallet inscribe --fee-rate <FEE_RATE> --batch "BATCH_FILE"`  
Example: `ord wallet inscribe --fee-rate 10 --batch "D:\Files\batch.yaml"`

[Batch Transaction](https://mempool.space/tx/a86a426fe273f330238765cd941477fa3f647dc9235cf36ba4c3e8b56064c335)
<details>
  <summary> Example .Yaml </summary>
  
```yaml
# there are two modes:
# - `separate-outputs`: place all inscriptions in separate postage-sized outputs
# - `shared-output`: place inscriptions in a single output separated by postage
mode: separate-outputs

# parent inscription:
parent: cadc6c906fcf340452c7ad40ce59dafb207b685026a18606531534f121d6c301i0

# `inscription`: path to inscription contents
# `metadata`: inscription metadata (optional)
# `metaprotocol`: inscription metaprotocol (optional)
inscriptions:
  - file: "D:/Inscriptions/Batch/1.txt"
    metadata:
      title: Batchie
      description: "1"

  - file: "D:/Inscriptions/Batch/2.txt"
    metadata:
      title: Batchie
      description: "2"

  - file: "D:/Inscriptions/Batch/3.txt"
    metadata:
      title: Batchie
      description: "3"

  - file: "D:/Inscriptions/Batch/4.txt"
    metadata:
      title: Batchie
      description: "4"

  - file: "D:/Inscriptions/Batch/5.txt"
    metadata:
      name: "Batchie"
      description: "A unique digital collectible from the Batchie series."
      edition: "5"
      attributes:
        - trait_type: "Background"
          value: "Bitcoin Orange"
        - trait_type: "Color"
          value: "BitGod Blue"
        - trait_type: "Accessory"
          value: "Fomoji Necklace"
        - trait_type: "Mood"
          value: "Contemplative"
      rarity: "Ultra Rare"

```
</details>

---

### 7 Inscribe on a specific sat (must have 2 utxos in wallet)
[Satpoint example transaction](https://mempool.space/tx/bf1af18d129f088353bb0ad37cdcf9f02b25e937583c366120da27eb7719b044#flow=&vin=0)  
`--satpoint <TXID:OUTPUT:SAT>` Create an inscription on a specific sat in your wallet.

Command: `ord wallet inscribe --fee-rate <FEE_RATE> --satpoint <TXID:OUTPUT:SAT> --file "FILE"`  
Example (10th sat in that utxo): `ord wallet inscribe --fee-rate 10 --satpoint 5ffab6b75dbfa7830a2d4a8520f13d6c7aa095688616af23c82a4da4c7af2e82:0:10 --file "D:\Files\1.txt"`

---

### 8 Start indexing with Satoshi tracking

Command: `ord --index-sats index run`
Explanation: `if you already have an index.redb then either rename it or delete it first`
