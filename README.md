# Invariant Testing Concepts in Solidity (Foundry)

Learning and exploration of **invariant testing** techniques inspired by [horsefacts/weth-invariant-testing](https://github.com/horsefacts/weth-invariant-testing).

---

## 📋 Table of Contents
- [Key Concepts Learned](#key-concepts-learned)
  - [1. Base Invariants](#1-base-invariants)
  - [2. Open vs Constrained Invariants](#2-open-vs-constrained-invariants)
  - [3. Bounded vs Unbounded Invariants](#3-bounded-vs-unbounded-invariants)
  - [4. Handler Contracts](#4-handler-contracts)
  - [5. Selectors](#5-selectors)
  - [6. Actors](#6-actors)
- [Summary of Testing Techniques](#summary-of-testing-techniques)
- [Resources](#resources)

---

## Key Concepts Learned

### 1. Base Invariants
✅ **Invariant tests** focus on system-wide properties that must always hold true, not just the result of a single function.

> Example: *"The total ETH in the Handler + WETH contract must always equal the initial ETH supply."*

---

### 2. Open vs Constrained Invariants
✅ **Open Invariants**: Allow the fuzzer to call any function with random inputs, simulating chaos.  
✅ **Constrained Invariants**: Structure the fuzzer by bounding inputs and scenarios for more realistic behaviors.

---

### 3. Bounded vs Unbounded Invariants
✅ **Bounded Invariants**: Use `bound()` to restrict fuzz inputs (e.g., deposits must not exceed balance).  
✅ **Unbounded Invariants**: Full randomization, higher exploration but with lots of noise (reverts).

---

### 4. Handler Contracts
✅ **Handlers** act as middlemen contracts that:
- Wrap the functions of the original contract under test.
- Allow cheatcodes like `vm.prank`, `deal`, etc.
- Simulate realistic environment setups.
- Introduce controlled randomness for testing.

---

### 5. Selectors
✅ **Selectors** allow you to choose specific contract functions for fuzzing instead of exposing the entire contract surface.

> Use `targetSelector` with precise function selectors to improve fuzz test focus and efficiency.

---

### 6. Actors
✅ **Actors** simulate different users interacting with the system (multi-address fuzzing):
- Tracked via a custom `AddressSet`.
- Functions like deposit, withdraw, transfer are fuzzed across many users.
- Introduces realism compared to all calls coming from just the Handler address.

---

## 📜 Summary of Testing Techniques

| Concept              | Purpose                                                         |
|----------------------|------------------------------------------------------------------|
| Invariant properties | Ensure critical system properties always hold.                  |
| Handlers             | Control interactions and environment for realistic fuzzing.     |
| Selectors            | Limit fuzzing to critical contract methods.                     |
| Actors               | Simulate a true multi-user environment.                         |
| Ghost Variables      | Track off-chain events like total deposits, withdrawals, etc.    |
| ForcePush            | Simulate `selfdestruct` scenarios that forcibly push ETH to contracts.|

---

## 📚 Resources

- Tutorial: [Invariant Testing Walkthrough (horsefacts)](https://github.com/horsefacts/weth-invariant-testing)
- Foundry Tools: [Foundry Book](https://book.getfoundry.sh/), Forge, Forge-std

---

> **Tip:**  
> Consider adding a `/test/handlers/` folder for handlers, using structured test file naming like `ContractName.invariants.t.sol` and applying `targetSelector` cleanly in `setUp()`.

---
