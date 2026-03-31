
<b>ChildERC20.sol</b>
```solidity
  function withdraw(uint256 amount) external {
        _burn(_msgSender(), amount);
    }
}
-----------------------------------------
 This function burns tokens of receiver.
------------------------------------------
```

<b>RootChainMananger.sol</b>

```solidity
  function exit(bytes calldata inputData) external override {
        ExitPayloadReader.ExitPayload memory payload = inputData.toExitPayload();

        bytes memory branchMaskBytes = payload.getBranchMaskAsBytes();
        uint256 blockNumber = payload.getBlockNumber();
        // checking if exit has already been processed
        // unique exit is identified using hash of (blockNumber, branchMask, receiptLogIndex)
        bytes32 exitHash = keccak256(
            abi.encodePacked(
                blockNumber,
                // first 2 nibbles are dropped while generating nibble array
                // this allows branch masks that are valid but bypass exitHash check (changing first 2 nibbles only)
                // so converting to nibble array and then hashing it
                MerklePatriciaProof._getNibbleArray(branchMaskBytes),
                payload.getReceiptLogIndex()
            )
        );

        require(
            processedExits[exitHash] == false,
            "RootChainManager: EXIT_ALREADY_PROCESSED"
        );
        processedExits[exitHash] = true;

        ExitPayloadReader.Receipt memory receipt = payload.getReceipt();
        ExitPayloadReader.Log memory log = receipt.getLog();

        // log should be emmited only by the child token
        address rootToken = childToRootToken[log.getEmitter()];
        require(
            rootToken != address(0),
            "RootChainManager: TOKEN_NOT_MAPPED"
        );
        if (migrationStatus[rootToken].isExitDisabled &&
            blockNumber > migrationStatus[rootToken].lastExitBlockNumber) { // @note: if the lastExitBlockNumber is the same as the current block number, exits are still allowed
            revert("RootChainManager: EXIT_DISABLED");
        }

        address predicateAddress = typeToPredicate[
            tokenToType[rootToken]
        ];

        // branch mask can be maximum 32 bits
        require(
            payload.getBranchMaskAsUint() &
            0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF00000000 ==
            0,
            "RootChainManager: INVALID_BRANCH_MASK"
        );

        // verify receipt inclusion
        require(
            MerklePatriciaProof.verify(
                receipt.toBytes(),
                branchMaskBytes,
                payload.getReceiptProof(),
                payload.getReceiptRoot()
            ),
            "RootChainManager: INVALID_PROOF"
        );

        // verify checkpoint inclusion
        _checkBlockMembershipInCheckpoint(
            payload.getBlockNumber(),
            payload.getBlockTime(),
            payload.getTxRoot(),
            payload.getReceiptRoot(),
            payload.getHeaderNumber(),
            payload.getBlockProof()
        );

        ITokenPredicate(predicateAddress).exitTokens(
            _msgSender(),
            rootToken,
            log.toRlpBytes()
        );
    }

```
