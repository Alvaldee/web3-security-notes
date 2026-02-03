# Ethernaut — Level 4: Telephone

## Summary
This challenge uses a contract using `tx.origin` for authorization instead of `msg.sender`, allowing attackers to bypass ownership checks by calling through an intermediate contract.

## Vulnerability
Incorrect identity verification using `tx.origin` instead of `msg.sender`.

## Root Cause
The contract assumes `tx.origin` reliably represents the authorized caller, ignoring the possibility of calls being routed through intermediary contracts.

## Exploit Strategy
1. Deploy an intermediate contract that calls the Telephone contract, making `tx.origin` the attacker's address while `msg.sender` is the intermediate contract, bypassing the ownership check.

## Impact
Unauthorised ownership change enabling complete contract control, fund theft, or contract destruction

## Mitigation
Always use `msg.sender` for access control and authentication, never use `tx.origin` except for very specific use cases like rejecting external contract interactions.

## Key Takeaway
`tx.origin` should never be used for authorization - it represents the transaction's starting point, not the immediate caller, creating a dangerous indirection vulnerability.
