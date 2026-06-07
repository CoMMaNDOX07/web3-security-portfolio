# S-156: transfer to contracts can permanently lock ETH

## Metadata

| Field | Details |
|---|---|
| Project | Garden |
| Platform | Code4rena |
| Severity | Low |
| Status | Valid |
| Code4rena Handle | CoMManDO |
| Submission ID | S-156 |
| Public Report Mapping | Low / Individual Submission |
| Contest Date | 24 Nov 2025 — 8 Dec 2025 |
| Report Date | 19 Feb 2026 |
| Ecosystem / Stack | Multichain, EVM, Solidity, Bitcoin Bridge |
| Affected File | `evm/src/swap/UDA.sol` |
| Affected Contracts | `UniqueDepositAddress`, `NativeUniqueDepositAddress` |
| Affected Function | `recover()` |
| Official Public Report | [Code4rena Report](https://code4rena.com/reports/2025-11-garden) |
| Contest Page | [Code4rena Audit Page](https://code4rena.com/audits/2025-11-garden) |
| Original Submission | [S-156](https://code4rena.com/audits/2025-11-garden/submissions/S-156) |
| Affected Code | [UDA.sol#L70](https://github.com/code-423n4/2025-11-garden/blob/main/evm/src/swap/UDA.sol#L70), [UDA.sol#L136](https://github.com/code-423n4/2025-11-garden/blob/main/evm/src/swap/UDA.sol#L136) |

## Summary

The `recover()` function sends the entire contract ETH balance to the configured refund address using Solidity’s `.transfer()`.

This is unsafe because `.transfer()` forwards only 2300 gas. If the refund address is a smart contract wallet, multisig, or any contract that needs more than 2300 gas in its `receive()` or fallback function, the transfer will revert.

As a result, ETH accidentally sent to the UDA contract can become permanently stuck.

## Vulnerability Details

The vulnerable pattern exists in both UDA contracts:

```solidity
function recover() public {
    (, address _refundAddress,,,,,) = getArgs();
    payable(_refundAddress).transfer(address(this).balance);
}
```

The issue is that `_refundAddress` is not guaranteed to be an externally owned account.

It can be:

- A Gnosis Safe or multisig
- A smart contract wallet
- A contract with custom receive logic
- Any arbitrary contract address

Many contract wallets perform logic inside `receive()` or `fallback()`, such as emitting events, checking state, updating storage, or forwarding funds. These operations may require more than 2300 gas.

Because `.transfer()` hardcodes the gas stipend, the call can fail forever for these recipients.

## Impact

ETH can become permanently locked in the UDA contract.

A realistic failure scenario:

1. The refund address is a smart contract wallet that requires more than 2300 gas to receive ETH.
2. ETH is accidentally sent to the UDA contract.
3. Someone calls `recover()`.
4. The `.transfer()` call reverts because the refund address cannot complete its receive logic within 2300 gas.
5. The ETH remains inside the UDA contract.
6. Every future call to `recover()` reverts for the same reason.

Since there is no alternative recovery path, the ETH can become irretrievable.

## Root Cause

The root cause is using:

```solidity
.transfer(address(this).balance)
```

to send ETH to an arbitrary refund address.

`.transfer()` is fragile because it forwards a fixed 2300 gas stipend and can fail for smart contract recipients.

## Recommended Mitigation

Replace `.transfer()` with a low-level `.call()` and check the return value.

Suggested fix:

```solidity
function recover() public {
    (, address _refundAddress,,,,,) = getArgs();

    (bool success, ) = payable(_refundAddress).call{value: address(this).balance}("");
    require(success, "ETH transfer failed");
}
```

Apply this fix to both affected `recover()` functions:

- `UniqueDepositAddress.recover()`
- `NativeUniqueDepositAddress.recover()`

If the contract later performs state changes around recovery, consider adding a reentrancy guard or following the checks-effects-interactions pattern.

## Proof of Concept Summary

The submitted scenario demonstrates the issue:

1. `refundAddress` is set to a smart contract wallet with non-trivial receive logic.
2. A user or contract accidentally sends ETH to the UDA.
3. `recover()` attempts to forward the ETH using `.transfer()`.
4. The refund contract’s `receive()` function runs out of gas.
5. The transfer reverts.
6. ETH remains locked in the UDA.
7. Repeated calls continue to revert because the same refund address and `.transfer()` behavior are used.

## References

- [Official Code4rena Report](https://code4rena.com/reports/2025-11-garden)
- [Code4rena Contest Page](https://code4rena.com/audits/2025-11-garden)
- [Original Submission S-156](https://code4rena.com/audits/2025-11-garden/submissions/S-156)
- [Affected Code: UDA.sol#L70](https://github.com/code-423n4/2025-11-garden/blob/main/evm/src/swap/UDA.sol#L70)
- [Affected Code: UDA.sol#L136](https://github.com/code-423n4/2025-11-garden/blob/main/evm/src/swap/UDA.sol#L136)
