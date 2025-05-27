
# STX-Reputex - Reputation System Smart Contract

This smart contract implements a **decentralized reputation system** on the Stacks blockchain using Clarity. It enables trusted interactions between participants in an auditing ecosystem through role-based access control, fungible reputation tokens, staking, and audit tracking. The contract is designed to incentivize and measure the performance of auditors through tokenized reputation, decaying metrics, and staking rewards.

---

## 🚀 Features

### 🧑‍⚖️ Auditor Management

* **Auditor Verification:** Only the contract owner can verify new auditors, capped at a maximum (`max-auditors`).
* **Auditor Removal:** Auditors can be removed by the contract owner.
* **Auditor Audit Tracking:** Each auditor can be audited and scored based on their performance.

### 🪙 Reputation Token System

* **Fungible Token:** `reputation-token` with a max supply of `1,000,000,000` units and 6 decimals.
* **Transfers & Burning:** Standard transfer and burn functionalities with safety checks for authorization and balance.
* **Token URI and Metadata:** Token includes a name, symbol, decimals, and optional URI.

### ⌛ Reputation Decay

* Reputation decays automatically over time:

  * **Decay Rate:** 10% per decay period.
  * **Decay Period:** Defined as one year (`52560` blocks assuming 10-minute block time).
  * **Decay Trigger:** Manually invoked via `decay-reputation`, only if enough blocks have passed.

### 📊 Audit Reports

* **Audit Submission:** Auditors can submit audit reports with a quality score based on completeness, accuracy, and timeliness.
* **Score Normalization:** Calculated via a weighted formula.
* **Audit Records:** Stored and accessible with status and timestamps.
* **Auditor Stats:** Tracks total audits, average score, and a reputation multiplier based on performance.

### 🔐 Role and Access Control

* **Contract Ownership:** Owner is the contract deployer (`tx-sender` at deployment).
* **Role Management:** Roles can be assigned via a roles map (`roles`) though extended role management is not included in this contract version.
* **Whitelisting:** Support for whitelisting principals.

### 🔁 Reputation Transfer

* Secure and validated reputation token transfers with memo support.
* Transfers update the `reputation-timestamps` to track when the last interaction occurred for decay calculations.

### 📉 Staking Functionality

* **Token Staking:** Allows users to lock reputation tokens for a given period.
* **Unstaking:** Users can withdraw their tokens after the lock period expires.
* **Rewards Calculation:** Based on a base annual reward rate (5%) and staking duration.

### 🧮 Utility and Read-Only Functions

* Query functions for:

  * Token metadata (name, symbol, decimals, URI)
  * Balance and total supply
  * Auditor stats and audit records
  * Staking positions and calculated rewards
  * Role and whitelist status
  * Decayed balances

---

## 🛡️ Constants and Limits

| Name               | Value         | Description                            |
| ------------------ | ------------- | -------------------------------------- |
| `max-auditors`     | 100           | Maximum allowed auditors               |
| `max-mint-amount`  | 1,000         | Cap on mintable amount (not used here) |
| `max-token-supply` | 1,000,000,000 | Maximum supply of reputation tokens    |
| `decay-rate`       | 10%           | Reputation decay per year              |
| `decay-period`     | 52,560 blocks | Approx. one year at 10 min block time  |
| `base-reward-rate` | 5%            | Base reward rate for staking           |

---

## ❗ Error Codes

| Code | Description               |
| ---- | ------------------------- |
| 101  | Not authorized            |
| 102  | Already an auditor        |
| 103  | Max auditors reached      |
| 104  | Mint limit exceeded       |
| 105  | Zero amount               |
| 106  | Insufficient balance      |
| 107  | Max token supply exceeded |
| 108  | Self-transfer not allowed |
| 109  | Decay period not reached  |
| 110  | Invalid audit score       |
| 111  | Invalid audit data        |
| 112  | Audit not found           |

---

## 🧠 Reputation Multiplier Logic

| Average Score | Multiplier |
| ------------- | ---------- |
| ≥ 90          | 1.5x       |
| ≥ 80          | 1.25x      |
| < 80          | 1x         |

---

## 📈 Reputation Quality Score Formula

```
quality-score = (completeness + 2 × accuracy + timeliness) / 4
```

---

## 📬 Events (Print Statements)

* `auditor_verified`
* `audit_submitted`
* `auditor-audited`
* `tokens_staked`
* `tokens_unstaked`

These events can be picked up by external indexers or UIs to provide visibility into contract actions.

---

## 🔐 Security Considerations

* All critical functions include `asserts!` for access control and input validation.
* Uses `safe-add` and `safe-subtract` functions to prevent overflow/underflow.
* Uses optional handling and unwraps carefully.
* No automatic token minting—controlled externally or via another governance layer.

---

## ✅ Future Enhancements

* **Role management modules**
* **Token minting logic**
* **Audit dispute mechanism**
* **Delegated staking or voting rights**

---

## 📜 License

This contract is open-source and intended for educational and experimental use. Please audit thoroughly before deploying in a production environment.

---
