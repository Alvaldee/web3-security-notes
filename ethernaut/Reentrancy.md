# Ethernaut — Level 10: Reentrancy

## Summary
This challenge demonstrates a reentrancy vulnerability where a contract sends Ether to an untrusted address before updating its internal state, allowing attackers to recursively drain funds through callback re-entry.

## Vulnerability
Unchecked external calls allowing recursive function re-entry before state updates.

## Root Cause
The withdraw function violates the checks-effects-interactions pattern by transferring Ether before updating user balances.

## Exploit Strategy
1. Deposit the minimum required funds into vulnerable contract
2. Trigger withdraw function for first payment
3. Fallback function automatically executes when receiving funds
4. Callback recursively calls withdraw function again
5. Continues until contract balance is drained

## Impact
Complete drainage of contract funds in a single transaction

## Mitigation
Follow the checks-effects-interactions pattern by updating internal state before making external calls.
Make users withdraw funds themselves instead of pushing payments.
OpenZeppelin's ReentrancyGuard with nonReentrant modifier

## Key Takeaway
Always update internal state before making external calls to untrusted addresses, as they can re-enter your function and exploit inconsistent state
