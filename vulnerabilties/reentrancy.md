# Reentrancy vulnerability

## Summary
`reentrancy` is a vulnerability where a contract can be used to execute multiple times before its state updates. This allows adversaries to reeneter the function and repeat the action, 
most notably withdrawing using the same state. Through such attacks, the advisory can withdraw all funds from the contract.

---

