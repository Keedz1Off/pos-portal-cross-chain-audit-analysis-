
<b>ChildERC20.sol</b>
```solidity
  function withdraw(uint256 amount) external {
        _burn(_msgSender(), amount);
    }
}

```
