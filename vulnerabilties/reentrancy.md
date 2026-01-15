# Reentrancy vulnerability

## Summary
`reentrancy` is a vulnerability where a contract can be used to execute multiple times before its state updates. This allows adversaries to reeneter the function and repeat the action, 
most notably withdrawing using the same state. Through such attacks, the advisory can withdraw all funds from the contract.

---

## Vulnerable Pattern
A common vulnerable pattern is a function makes an external call before updating its own important state, allowing the function to be called repetitively.

```solidity
function withdraw() external {
        uint256 amount = balances[msg.sender];
        require(amount > 0);

        (bool ok, ) = msg.sender.call{value: amount}("");
        require(ok, "Send failed");

        balances[msg.sender] = 0;
    }
```

