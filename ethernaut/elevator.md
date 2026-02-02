# Ethernaut — Level 11: Elevator

## Summary
This challenge demonstrates how trusting an untrusted contract to implement an interface can allow attackers to violate function state assumptions and manipulate contract logic.

## Vulnerability
Unsafe reliance on external interface return values across multiple calls.

## Root Cause
The Elevator contract violates a state consistency assumption by treating isLastFloor() as a stateless view function. However, attackers can implement it with internal state and return different values on successive calls.

## Exploit Strategy
1. Implement isLastFloor() to return false then true
2. Trigger the Elevator's function
3. Return false to pass the initial check
4. Return true to set top = true
5. Reach the "top" without actually being on last floor

## Impact
Control contract state through malicious callbacks
 
## Mitigation
Avoid calling untrusted external functions multiple times with the same parameters. Cache and reuse return values instead of re-invoking external calls, and avoid making logic decisions based on assumptions about external contract behavior.

## Key Takeaway
Never assume external function calls are stateless as attackers can and will use stateful implementations to return different values on subsequent calls, breaking logic flow.
