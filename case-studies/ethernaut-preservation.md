# Ethernaut: Preservation — Delegatecall Storage Collision

## Summary
This case study analyzes the Ethernaut *Preservation* challenge, which
demonstrates how improper use of `delegatecall` can lead to storage corruption
and ownership takeover.

The vulnerability arises from delegating execution to a library contract whose
assumed storage layout does not match the calling contract.

---

## Vulnerability
The contract uses `delegatecall` to invoke a library function intended to update
a timestamp value. However, because `delegatecall` executes code in the storage
context of the calling contract, any state writes in the library affect the
caller’s storage slots.

The library assumes its own storage layout, which results in storage slot
misalignment when executed via `delegatecall`.

---

## Root Cause
- `delegatecall` preserves the caller’s storage
- The library writes to storage slot `0`
- Slot `0` in the calling contract stores the library address
- No validation exists to ensure storage layout alignment

This allows an attacker to overwrite the library address with a malicious
contract.

---

## Exploitation Overview
The attack occurs in two stages:

1. **Library Replacement**
   - The attacker calls the vulnerable function with a crafted input
   - Storage slot `0` is overwritten with the attacker-controlled contract address

2. **Ownership Takeover**
   - A second call triggers `delegatecall` to the attacker’s contract
   - The malicious contract writes to the storage slot corresponding to `owner`
   - Ownership of the contract is transferred to the attacker

---

## Impact
Successful exploitation allows an attacker to:
- Take ownership of the contract
- Execute privileged functions
- Permanently compromise contract integrity

In a real-world deployment, this could result in loss of funds or protocol
control.

---

## Real-World Relevance
This vulnerability pattern appears in:
- Unsafe proxy implementations
- Misconfigured upgradeable contracts
- Contracts using external libraries without fixed storage layouts

Similar issues have caused production exploits in multiple DeFi protocols.

---

## Mitigation
- Avoid `delegatecall` for simple logic reuse
- Use immutable libraries where possible
- Follow standardized proxy patterns (e.g. EIP-1967)
- Ensure strict storage layout alignment
- Restrict access to functions that invoke `delegatecall`

---

## Key Takeaway
`delegatecall` is not just a function call — it is a context switch. Any misuse
can turn external code execution into a full contract takeover.
