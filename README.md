# ⛓️ Layer2 Rollup Factory – Bitcoin Scaling Solution

A smart contract for deploying and managing **Layer 2 rollup chains** designed to scale the Bitcoin ecosystem via customizable optimistic or zk-based rollups. Includes fraud-proof challenge mechanism, user deposit/withdrawal flows, and NFT-based operator governance.

---

## 🌐 Why This Factory?

- ⚡ Bitcoin Layer 2 scalability with low fees and high throughput  
- 🛠️ Customizable rollups: block time, tx count, execution style  
- 🔐 Fraud-proof submission and slashing mechanism  
- 💸 User deposits/withdrawals with finality lock  
- 🧾 NFT minting for verified rollup operators  
- 📈 Designed for long-term BTC growth scenarios (e.g., $180k BTC thesis)

---

## 🔧 Rollup Creation & Config

```clojure
(create-rollup name block-time max-tx-per-block data-availability execution-type)
````

| Parameter           | Description                      |
| ------------------- | -------------------------------- |
| `block-time`        | Min 6 (approx. 1 minute)         |
| `max-tx-per-block`  | Capped at 1000                   |
| `data-availability` | `onchain`, `ipfs`, or `celestia` |
| `execution-type`    | `optimistic` or `zk`             |

✅ Operator must bond **100 STX** to register
✅ Operator receives **NFT ID** tied to the rollup

---

## 🔁 Transaction Flow

### 🔹 Submit Rollup Batch

```clojure
(submit-batch rollup-id new-state-root tx-count data-hash)
```

* Only the operator of the rollup can submit
* Batch is open to **challenge** for \~2 weeks (2016 blocks)
* Stores state root and tx hash for verification

---

### 🔸 Deposit Funds

```clojure
(deposit-to-rollup rollup-id amount)
```

* Transfers STX to the contract for rollup usage
* Increments total value locked (TVL)

---

### 🔸 Initiate Withdrawal

```clojure
(initiate-withdrawal rollup-id amount merkle-proof)
```

* Requires inclusion proof (buff)
* Withdrawal executes only after **finality period (\~4 weeks)**

---

### 🔸 Execute Withdrawal

```clojure
(execute-withdrawal rollup-id request-id)
```

* Can only be called after `execution-block`
* Verifies user and updates deposit record

---

## ⚠️ Fraud Proof System

### 🔺 Challenge a Batch

```clojure
(challenge-batch rollup-id batch-id fraud-proof)
```

* Can be called by **any user** within the challenge window
* Locks a **50 STX bond** from challenger
* Challenge is resolved after 144 blocks (\~1 day)

### 🔹 Finalize a Batch

```clojure
(finalize-batch rollup-id batch-id)
```

* Marks batch as final if no fraud challenge occurred or time expired

---

## 📦 Key Data Structures

| Name                  | Description                                      |
| --------------------- | ------------------------------------------------ |
| `rollups`             | All active rollups and configurations            |
| `rollup-batches`      | Submitted batches with roots, hashes, and status |
| `fraud-challenges`    | Fraud reports and dispute bonds                  |
| `user-deposits`       | Balances and pending withdrawals                 |
| `withdrawal-requests` | Time-locked withdrawal queue                     |

---

## 🧠 Read-Only Utilities

```clojure
(get-rollup rollup-id)
(get-batch rollup-id batch-id)
(get-user-balance rollup-id user)
(calculate-rollup-tvl rollup-id)
```

---

## ⚙️ Internal Logic (Dev Notes)

* `verify-fraud-proof`: Placeholder that returns `true` by default — extend with actual validation logic (e.g., zkSNARK checks)
* `slash-operator`: Halts malicious rollups and deactivates the operator

---

## 🔐 Access & Roles

| Action             | Role                      |
| ------------------ | ------------------------- |
| Create rollup      | Anyone with 100 STX       |
| Submit batch       | Rollup operator           |
| Challenge batch    | Any user (bond required)  |
| Finalize batch     | Anyone (post-deadline)    |
| Execute withdrawal | Only withdrawal initiator |

---

## 📊 Security & Assumptions

* STX is used as both operational capital and anti-fraud collateral
* Merkle proofs must be validated off-chain or via future on-chain logic
* Actual **fraud-proof logic** should be implemented in `verify-fraud-proof`

---

## 📈 Roadmap Ideas

* Add zkSNARK/zk-STARK fraud proof integration
* Dynamic bonding curves for operator tiers
* Multi-asset rollup deposits with fungible token support
* Sequencer rotation and governance upgrades

---

## 🪙 License

MIT – Open source, scalable Bitcoin infrastructure.

---

## 🧪 Inspired By

* Optimism & Arbitrum fraud challenge model
* Celestia modular data availability
* Ethereum rollup factory concepts applied to Bitcoin
