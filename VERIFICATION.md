# How to Verify a Sovereign Artefact Registry Anchor

This document provides the exact steps for any third party to independently verify a cryptographic anchor published by the Sovereign Artefact Registry.

## Prerequisites

- Python 3.8+ and `pip`
- Git
- An internet connection

## Step 1: Install OpenTimestamps Client

Open your terminal (Linux/macOS) or PowerShell (Windows) and run:

```bash
pip install opentimestamps-client
```

## Step 2: Clone the Public Anchor Repository

```bash
git clone https://github.com/Andy160675/registry-anchors-public.git
cd registry-anchors-public
```

## Step 3: Choose an Anchor to Verify

Each client has its own directory under `public/`. For this example, we will verify the first anchor for `css_internal`.

- **Anchor file**: `public/css_internal/2026-02-28T16-00-32Z.root.ots`
- **Record file**: `public/css_internal/anchors.jsonl`

## Step 4: Verify the OpenTimestamps Proof

This command contacts public calendar servers to verify the timestamp is anchored in the Bitcoin blockchain.

```bash
ots verify public/css_internal/2026-02-28T16-00-32Z.root.ots
```

**Expected Output (after Bitcoin confirmation):**

```
Success! Bitcoin block 832041 attests data existed as of 2026-02-28T16:00:32Z UTC
```

*Note: If the anchor is recent, the output may show "Pending confirmation". This is normal. Verification is complete once a Bitcoin block is cited.*

## Step 5: Verify the Cryptographic Hashes

This step ensures the proof file you verified (`.ots`) matches the record in the `anchors.jsonl` log.

### A. Find the `ots_hash` in the log

Open `public/css_internal/anchors.jsonl` and find the record with the matching `ots_file` name. The `ots_hash` for this anchor is:

> `573ebe29343a091f12f93e2ff8275ec71b6f80675c72aa97bbf7666d2f08a4ec`

### B. Compute the SHA-256 hash of the `.ots` file

**Linux/macOS:**
```bash
sha256sum public/css_internal/2026-02-28T16-00-32Z.root.ots
```

**Windows PowerShell:**
```powershell
Get-FileHash .\public\css_internal\2026-02-28T16-00-32Z.root.ots -Algorithm SHA256
```

### C. Compare the hashes

The hash computed by your machine **must exactly match** the `ots_hash` from the `anchors.jsonl` file. This proves the integrity of the record.

---

## What This Proves

- The registry state existed at the attested time.
- It has not been modified since anchoring.
- The proof is independently verifiable via the Bitcoin blockchain.
- No party (including the registry operator) can alter history without detection.
