# Ethernaut — Level 13: Gatekeeper One

## Summary
A contract with three restrictive gate conditions requiring precise gas, data type, and caller address calculations to bypass.

## Vulnerability
Fragile access control mechanisms based on gas constraints and bitwise checks.

## Root Cause
The contract's security relies on gas calculations and address bitmasks that can be reverse-engineered through brute-force testing and precise transaction crafting.

## Exploit Strategy
1. Deploy attacker contract to bypass Gate 1
2. Brute-force gas calculation for Gate 2 by testing gas amounts
3. Bitmask address crafting for Gate 3 using address manipulation
4. Send precise transaction meeting all three conditions simultaneously

## Impact
Unauthorised entry and potential contract takeover by bypassing all three security gates through systematic analysis.

## Mitigation
Use robust, non-predictable access controls like signature verification or established permission systems; avoid security through gas or address-based obscurity.

## Key Takeaway
Never rely on gas calculations or address bitmasking for security - these are predictable and reversible through brute-force testing, not cryptographic guarantees.
