
# Ethernaut — Level 14: Gatekeeper Two

## Summary
A contract using assembly code with `extcodesize` checks that can be bypassed by calling from a constructor when contract code size is zero

## Vulnerability
Improper contract caller detection using `extcodesize`.

## Root Cause
Using `extcodesize` check in modifier that fails during contract construction, enabling attackers to call from constructor.

## Exploit Strategy
1. Deploy attack contract with malicious call in constructor
2. During construction, `extcodesize` returns 0, bypassing the check
3. Execute the restricted function before contract deployment completes
4. Pass the XOR bitmask requirement with address calculation

## Impact
Unauthorised access to protected functions, potentially enabling ownership takeover or privilege escalation.

## Mitigation
Use established access control patterns, avoid assembly-level checks for authorization, and implement comprehensive multi-layered security.

## Key Takeaway
Assembly-level security checks can be bypassed by understanding EVM execution context `extcodesize` is 0 during construction, creating a temporary window for attack.
