# Delegatecall Misuse

## Summary
`delegatecall` allows a contract to execute code from another contract while
preserving the storage, msg.sender, and msg.value of the calling contract.
If used incorrectly, this can lead to storage corruption and privilege
escalation.

Improper delegatecall usage is a common source of critical vulnerabilities in
upgradeable and library-based smart contract designs.

---

## Vulnerable Pattern
A common vulnerable pattern is delegating execution to an external contract
without strict control over the implementation address or storage layout.

```solidity
contract Victim {
    address public library;
    address public owner;

    function set(uint256 value) external {
        library.delegatecall(
            abi.encodeWithSignature("set(uint256)", value)
        );
    }
}

