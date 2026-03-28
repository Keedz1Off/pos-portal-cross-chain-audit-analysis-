```rust

   function _depositEtherFor(address user) private {
        bytes memory depositData = abi.encode(msg.value);
        _depositFor(user, ETHER_ADDRESS, depositData);

        // payable(typeToPredicate[tokenToType[ETHER_ADDRESS]]).transfer(msg.value);
        // transfer doesn't work as expected when receiving contract is proxified so using call
        (bool success, /* bytes memory data */) = typeToPredicate[tokenToType[ETHER_ADDRESS]].call{value: msg.value}("");
        if (!success) {
            revert("RootChainManager: ETHER_TRANSFER_FAILED");
        }
    }

    function _depositFor(
        address user,
        address rootToken,
        bytes memory depositData
    ) private {
        if (migrationStatus[rootToken].isDepositDisabled) {
            revert("RootChainManager: DEPOSIT_DISABLED");
        }
        bytes32 tokenType = tokenToType[rootToken];
        require(
            rootToChildToken[rootToken] != address(0x0) &&
               tokenType != 0,
            "RootChainManager: TOKEN_NOT_MAPPED"
        );
        address predicateAddress = typeToPredicate[tokenType];
        require(
            predicateAddress != address(0),
            "RootChainManager: INVALID_TOKEN_TYPE"
        );
        require(
            user != address(0),
            "RootChainManager: INVALID_USER"
        );

        ITokenPredicate(predicateAddress).lockTokens(
            _msgSender(),
            user,
            rootToken,
            depositData
        );
        bytes memory syncData = abi.encode(user, rootToken, depositData);
        _stateSender.syncState(
            childChainManagerAddress,
            abi.encode(DEPOSIT, syncData)
        );
    }
```
