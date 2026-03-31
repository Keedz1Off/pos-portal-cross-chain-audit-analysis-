```solidity
    function onStateReceive(uint256, bytes calldata data)
        external
        override
        only(STATE_SYNCER_ROLE)
    {
        (bytes32 syncType, bytes memory syncData) = abi.decode(
            data,
            (bytes32, bytes)
        );

        if (syncType == DEPOSIT) {
            _syncDeposit(syncData);
        } else if (syncType == MAP_TOKEN) {
            (address rootToken, address childToken, ) = abi.decode(
                syncData,
                (address, address, bytes32)
            );
            _mapToken(rootToken, childToken);
        } else {
            revert("ChildChainManager: INVALID_SYNC_TYPE");
        }
    }
     emit TokenUnmapped(rootToken, childToken);
    }

```

Wht this function actually do?
----------------------------------------------------------
1.If  syncData is "DEPOSIT" mints tokens to user on L2.
------------------------------------------------------------------
2.If syncData is "MAP_TOKEN" registers tokens on L2.
---------------------------------------------
3.If anything else, revert(INVALID_SYNC_TYPE).
---------------------------------------------
 <b>Invariants: </b> ❗❗❗

syncType must be or "DEPOSIT" or "MAP_TOKEN", otherwise revert.
-----------------------------------------------------------------
<b>vulnerabilitys:</b>❗❗❗

|explication| vulnerability | solution |
|-----------|-----------|-----------|
 | Use of devices that should not be available, that is STATE_SYNCER_ROLE capabilities used by a hacker.| Centralization Risk  | only(STATE_SYNCER_ROLE)|
|The contract receives "garbage" (invalid data type), but instead of throwing an error (revert), it completes the transaction with the Success status.  |Silent Failure| if..., else if..., else revert("...")  |



------------------------------
DEPOSIT FLOW FINISHED
-----------------------------
<b>next:</b>
<a href="">WITHDRAW</a>
