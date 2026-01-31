# Ethernaut — Level 6: Delegate

## Summary
This challenge demonstrates how using delegatecall inside a fallback function can allow an attacker to execute privileged library functions in the context of the calling contract. 
Although the main contract does not expose a pwn() function, untrusted calldata can be forwarded to a library contract, resulting in ownership takeover.

## Vulnerability
Privilege escalation via unauthenticated delegatecall in fallback function.

## Root Cause
The fallback function blindly forwards all unknown function calls via delegatecall to a library contract. 
Because delegatecall executes code in the storage context of the calling contract, any state changes performed by the library affect the caller’s storage, including sensitive variables such as ownership.

## Exploit Strategy
1. Craft calldata corresponding to the pwn() function defined in the library contract.
2. Send the calldata directly to the main contract, triggering its fallback function.
3. The fallback function delegatecalls into the library, executing pwn() in the caller’s storage context and setting the attacker as owner.

## Impact
Attacker gains full ownership and enables complete administrative control.

## Mitigation
Avoid forwarding arbitrary calldata via delegatecall in fallback functions. If delegatecall is required, strictly limit callable functions and ensure the library code cannot modify sensitive state variables such as ownership.

## Key Takeaway
delegatecall executes external code as if it were part of the calling contract. 
Using it in fallback functions without strict controls effectively exposes the contract’s internal state to arbitrary external callers.
