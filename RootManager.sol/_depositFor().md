```solidity
function depositFor(
    address user,
    address rootToken,
    bytes calldata depositData
) external override {
    require(
        rootToken != ETHER_ADDRESS,
        "RootChainManager: INVALID_ROOT_TOKEN"
    );
    _depositFor(user, rootToken, depositData);
}
```



What this function actually do?

This function is the first step in sending tokens to Polygon. It stores all transaction data and ensures that the provided token is registered and valid for deposit.
-----------------------------------------------------------
Invariants: All data in the parameter always must be true.
---------------------------------------------------------------


<img width="523" height="446" alt="image" src="https://github.com/user-attachments/assets/e34e6037-8581-4674-aa21-8277f4fc32e1" />












