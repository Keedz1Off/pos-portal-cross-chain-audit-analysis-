
```solidity
    function lockTokens(
        address depositor,
        address depositReceiver,
        address rootToken,
        bytes calldata depositData
    )
        external
        override
        only(MANAGER_ROLE)
    {
        uint256 amount = abi.decode(depositData, (uint256));
        emit LockedERC20(depositor, depositReceiver, rootToken, amount);
        IERC20(rootToken).safeTransferFrom(depositor, address(this), amount);
    }
```

What this function actually do?
The function moves the user's tokens to the predicate contract,
thereby locking them on Ethereum before the Polygon mint.

Invariants:
This function can be called only by MANAGE_ROLE;

