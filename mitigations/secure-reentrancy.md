# Secure Usage of reentrancy

## Overview
`reentrancy` is a vulnerability that occurs when a contract performs an external call before updating its internal state which allows continous execution. Which can lead to the contract having all its funds drained by the adversary.

This document outlines best practices to safely use `reentrancy` in production smart contracts.

---

## Common Risks
- Finiancial loss
- State desynchronisation
- Unauthorised actions
- Complete loss of contract ownership

---

## Mitigation Strategies

