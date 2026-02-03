# Ethernaut — Level 16: Preservation

## Summary
This challenge demonstrates how using `delegatecall` with user-influenced library addresses can lead to storage slot collisions, allowing attackers to overwrite critical contract state such as ownership.

## Vulnerability
Storage slot collision via unsafe delegatecall enabling full contract takeover.

## Root Cause
The contract uses `delegatecall` to externally supplied library addresses without enforcing a fixed or trusted storage layout. Because delegatecall executes in the caller’s storage context, storage writes performed by the library overwrite variables in the main contract.

## Exploit Strategy
1. Deploy a malicious contract with a storage layout aligned to the caller’s critical variables (e.g. owner slot).
2. Call `setFirstTime()` to overwrite the library address through a storage collision.
3. Call `setFirstTime()` again, now delegatecalling into the malicious contract.
4. Malicious code executes in the caller’s context and sets the owner to the attacker’s address.

## Impact
Complete ownership takeover, granting the attacker full administrative control over the contract.

## Mitigation
Use stateless libraries without storage variables, restrict delegatecall targets to trusted and immutable implementations, or adopt standardized proxy patterns with fixed and audited storage layouts.

## Key Takeaway
Using delegatecall with user-controlled or mutable library addresses enables arbitrary state modification through storage collisions, often resulting in full contract compromise.
