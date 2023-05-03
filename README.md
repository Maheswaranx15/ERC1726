# ERC1726
A mintable dividend-paying ERC20 token contract written in solidity.


---
eip: <to be assigned>
title: Dividend-Paying Token Standard
author: Roger Wu (@Roger-Wu) <gsetcrw@gmail.com>, Tom Lam (@erinata) <tomlam@uchicago.edu>
type: Standards Track
category: ERC
status: Draft
created: 2019-01-27
---

## Simple Summary

A standard interface for dividend-paying tokens.


## Abstract

The following standard allows for the implementation of a standard API for dividend-paying tokens within smart contracts.
This standard provides basic functionality to allow anyone to pay and distribute ether to all token holders.
A token holder can then view or withdraw the ether distributed to them from the contract.


## Motivation

While the number of tokens and dapps with dividend paying features is growing, there is few widely adopted standard interfaces for such contracts.
A standard interface allows wallet/exchange applications to work with any dividend-paying token on Ethereum, which makes it much easier to view and manage dividends from different sources.

In this EIP, we propose a standard interface and a well-tested implementation for a dividend-paying token contract.
We hope that our work could help the community to reach a consensus of the standard.


## Specification

```solidity
pragma solidity ^0.5.0;


/// @title Dividend-Paying Token Interface
/// @author Roger Wu (https://github.com/roger-wu)
/// @dev An interface for a dividend-paying token contract.
interface DividendPayingTokenInterface {
  /// @notice View the amount of dividend in wei that an address can withdraw.
  /// @param _owner The address of a token holder.
  /// @return The amount of dividend in wei that `_owner` can withdraw.
  function dividendOf(address _owner) external view returns(uint256);

  /// @notice Distributes ether to token holders as dividends.
  /// @dev SHOULD distribute the paid ether to token holders as dividends.
  ///  SHOULD NOT directly transfer ether to token holders in this function.
  ///  MUST emit a `DividendsDistributed` event when the amount of distributed ether is greater than 0.
  function distributeDividends() external payable;

  /// @notice Withdraws the ether distributed to the sender.
  /// @dev SHOULD transfer `dividendOf(msg.sender)` wei to `msg.sender`, and `dividendOf(msg.sender)` SHOULD be 0 after the transfer.
  ///  MUST emit a `DividendWithdrawn` event if the amount of ether transferred is greater than 0.
  function withdrawDividend() external;

  /// @dev This event MUST emit when ether is distributed to token holders.
  /// @param from The address which sends ether to this contract.
  /// @param weiAmount The amount of distributed ether in wei.
  event DividendsDistributed(
    address indexed from,
    uint256 weiAmount
  );

  /// @dev This event MUST emit when an address withdraws their dividend.
  /// @param to The address which withdraws ether from this contract.
  /// @param weiAmount The amount of withdrawn ether in wei.
  event DividendWithdrawn(
    address indexed to,
    uint256 weiAmount
  );
}
```

OPTIONAL functions for a dividend-paying token contract:

```solidity
pragma solidity ^0.5.0;


/// @title Dividend-Paying Token Optional Interface
/// @author Roger Wu (https://github.com/roger-wu)
/// @dev OPTIONAL functions for a dividend-paying token contract.
interface DividendPayingTokenOptionalInterface {
  /// @notice View the amount of dividend in wei that an address can withdraw.
  /// @param _owner The address of a token holder.
  /// @return The amount of dividend in wei that `_owner` can withdraw.
  function withdrawableDividendOf(address _owner) external view returns(uint256);

  /// @notice View the amount of dividend in wei that an address has withdrawn.
  /// @param _owner The address of a token holder.
  /// @return The amount of dividend in wei that `_owner` has withdrawn.
  function withdrawnDividendOf(address _owner) external view returns(uint256);

  /// @notice View the amount of dividend in wei that an address has earned in total.
  /// @dev accumulativeDividendOf(_owner) = withdrawableDividendOf(_owner) + withdrawnDividendOf(_owner)
  /// @param _owner The address of a token holder.
  /// @return The amount of dividend in wei that `_owner` has earned in total.
  function accumulativeDividendOf(address _owner) external view returns(uint256);
}
```

## Implementation

Our implementation of a mintable dividend-paying ERC20 token: https://github.com/Roger-Wu/DividendPayingToken


## Backwards Compatibility

The interface is designed to be compatible with ERC20 tokens.


## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).