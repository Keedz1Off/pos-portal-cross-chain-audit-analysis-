

<img width="1024" height="500" alt="image" src="https://github.com/user-attachments/assets/ce23cf9d-f4d4-4970-847a-df65e9a15b8a" />

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
<b>What this function actually do?</b>

1.Checks whether deposits are disabled 
------------------------------------------
2.Checks whether the token is registered
-----------------------------------------
3.Ensures the token type matches a valid predicate
-----------------------------------------------
4.Ensures the user address is valid 
-----------------------------------------------
5.Calls the predicate contract to lock tokens 
---------------------------------------------------
Invariants:

rootToChildToken[rootToken] != 0 && tokenToType[rootToken] != 0
---------------------------------------------------------------
typeToPredicate[tokenType] != 0
---------------------------------------------------------
user != address(0)
ITokenPredicate.lockTokens() must succeed
----------------------------------------------------------
 _stateSender.syncState() must be called after successful lockTokens
------------------------------------------------------------------------------

Vurnerabillitys  :

<img width="809" height="258" alt="image" src="https://github.com/user-attachments/assets/a0fbb675-26f1-4ad6-b474-4ee2b210a140" />

<img width="1536" height="250" alt="ChatGPT Image 29 de març del 2026, 21_46_30" src="https://github.com/user-attachments/assets/75447793-3a0f-434a-a858-958d24bfd0ba" />




 


