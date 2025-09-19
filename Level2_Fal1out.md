# Ethernaut Level – Fal1out Contract Audit

## Summary

The Fal1out contract contains a **constructor mis-declaration vulnerability**. The developer mistakenly defined a function `Fal1out()` instead of using the `constructor` keyword. An attacker can call `Fal1out()` after deployment to **become the owner**, allowing them to withdraw all contract funds.

---
## How to Audit Fal1out on Ethernaut via Console Interaction

```js
contract.Fal1out()
```

1. `contract.Fal1out()`
   This directly calls the **Fal1out function**, which was mistakenly written as a regular function instead of a constructor.

   * When executed, it sets `owner = msg.sender`.
   * This makes you (the caller) the new **owner** of the contract.

## Vulnerability Details

* **Functions affected:** Fal1out()
* **Type:** Ownership hijack / access control bypass
* **Problem:**
  The developer intended `Fal1out()` to act as the constructor, setting the initial owner. However, because it is a **public function** in Solidity >=0.6.0, it can be called multiple times.

```solidity
function Fal1out() public payable {
    owner = msg.sender;
    allocations[owner] = msg.value;
}
```

This allows anyone who calls the function to overwrite the owner, bypassing intended ownership protections.

---

## Steps to Reproduce

1. Deploy the Fal1out contract.
2. Call the `Fal1out()` function from any address.
3. Verify that ownership has changed to your address:

```solidity
owner(); // returns attacker address
```

4. Withdraw contract funds using `collectAllocations()` (on test deployment only).

---

## Impact

* **Severity:** Critical
* **Consequences:** Full control of the contract and access to all funds.
* **Observation:** Static analyzers may not flag this because it relies on **logical reasoning about function vs constructor mis-declaration**, not just code syntax.

---

## Mitigation

* Use the **constructor keyword** instead of a function with the contract name:

```solidity
constructor() payable {
    owner = msg.sender;
    allocations[owner] = msg.value;
}
```

* Ensure critical variables such as ownership and balances **cannot be modified by public functions** after deployment.

---

## Notes

* Tested locally on Remix.
* Tool used: Remix basic analyzer.
* This audit highlights the importance of **using constructors correctly** to prevent ownership hijack vulnerabilities.

---
