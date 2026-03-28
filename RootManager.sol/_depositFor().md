```solidity

  
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
            abi.encode(DEPOSIT, syncData)    }
```
What this function actually do?

Checks whether deposits are disabled
Checks whether the token is registered
Ensures the token type matches a valid predicate
Ensures the user address is valid
Calls the predicate contract to lock tokens

Invariants:
ootToChildToken[rootToken] != 0 && tokenToType[rootToken] != 0
typeToPredicate[tokenType] != 0
user != address(0)
ITokenPredicate.lockTokens() must succeed
 _stateSender.syncState() must be called after successful lockTokens

Vurnerabillity:
 
