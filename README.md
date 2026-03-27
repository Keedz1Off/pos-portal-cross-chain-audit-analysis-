
<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/c3dac860-71b0-45b4-893f-d4783ef3c703" />

# Smart Contract Audit Journey

## 👋 About Me(im 15 btw)

I am a beginner smart contract auditor, focused on learning blockchain security and Web3 architecture.

---

## 📚 About This Repository

This repository is dedicated to analysis and research of existing smart contracts.

I do not write my own contracts here. Instead, I:

* Study real-world smart contract code
* Break down protocol logic and architecture
* Analyze how contracts interact with each other
* Explore real use cases from production systems
* Document my notes and understanding

---

## 🔍 What I Do

* Step-by-step analysis of functions and flows (deposit / withdraw, etc.)
* Understanding contract behavior in different scenarios
* Studying security mechanisms
* Identifying potential vulnerabilities and edge cases
* Exploring complex systems like cross-chain bridges

---

## 🔬 Current Focus

* Polygon PoS Bridge (RootChainManager)
* Cross-chain interactions
* Merkle proof verification
* Deposit / Exit logic
* Smart contract security fundamentals

---

## 🎯 Goal

My goal is to become a skilled smart contract auditor by deeply understanding real-world protocols and their design.

---

## 🚀 Progress

This repository will be continuously updated as I analyze more projects and improve my skills.

---






# Smart Contract Audit Study – Index

## 1. RootChainManager.sol (Ethereum)

* **depositFor(user, rootToken, depositData)** – main deposit entry
* **_depositFor(...)** – internal deposit logic
* Later: `_syncState`, `_mapToken`

## 2. Predicate Contracts

* **ERC20Predicate  – token-specific handling on deposit

## 3. Child Tokens (Polygon)

* **ChildERC20 – mint & burn on Polygon

## 4. Withdraw / Exit

* **exit(bytes inputData)** – user exit
* `_processExit` – proof verification, double exit protection

---

**Notes:**

* Start: `RootChainManager.sol → depositFor → _depositFor`
* Focus: logic, flow, security, interactions
* No new code is written, only analysis

---

