
# Ethernaut Level 1 – Fallback Contract Audit

## Summary
The Fallback contract contains an **ownership takeover vulnerability**. An attacker with a minimal contribution can bypass the ownership logic in `contribute()` by using the `receive()` function and become the owner, allowing them to withdraw all contract funds.

## Vulnerability Details
- **Functions affected:** `contribute()`, `receive()`  
- **Type:** Ownership hijack / access control bypass  
- **Problem:**  
  The `contribute()` function restricts contributions to less than 0.001 ether, so an attacker cannot out-contribute the initial owner (1000 ether).  
  The `receive()` function only checks that the sender has a non-zero contribution:

```solidity
receive() external payable {
    require(msg.value > 0 && contributions[msg.sender] > 0);
    owner = msg.sender;
}

````
This allows anyone with a minimal contribution to become the owner, bypassing the intended ownership logic.

## Steps to Reproduce

1. Deploy the Fallback contract.
2. Make a minimal contribution (e.g., 1 wei) from your address.
3. Send any ETH to the contract to trigger `receive()`.
4. Verify that ownership has changed to your address.
5. Withdraw the contract balance (on test deployment only).

## Impact

* **Severity:** Critical
* **Consequences:** Full control of the contract and funds.
* **Observation:** Static analyzers may not flag this because it requires reasoning about ownership logic, not just code syntax.

## Mitigation

* Restrict ownership assignment in `receive()`:

```solidity
require(contributions[msg.sender] > contributions[owner]);
```

* Or remove ownership assignment from `receive()` entirely and rely on `contribute()` for ownership changes.

## How to audit on fallback level via console interaction

```js
await contract.contribute({value: toWei("0.0002")})
contract.sendTransaction({value: toWei("0.003")})
contract.withdraw()
```

1. `await contract.contribute({value: toWei("0.0002")})`
   This function deposits **0.0002 ether** to the contract by converting ether → wei (the EVM unit). 

2. `contract.sendTransaction({value: toWei("0.003")})`
   This sends **0.003 ether** directly to the contract address. If the contract has a `receive()`/fallback, that function will automatically run and accept the 0.003 ether.

3. `contract.withdraw()`
   This calls the contract’s withdraw function and — if the caller is the `owner` (i.e., you have already gained ownership) — it will transfer the contract’s entire balance to the owner.


---

**Notes:**

* Tested locally on Remix.
* Tool used: Remix basic analyzer.
* This audit highlights how `fallback` or `receive` functions can bypass balance-based logic in contracts.
