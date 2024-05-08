# Stake ETH on Frax

The Frax Stake adapter outlines a feature that enables users holding funds on any compatible chain to stake ETH on Frax in one step.

## Components of the Frax Stake Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, fraxETH, staked FraxETH and the address of the Frax ETH Minter for the respective chain.

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data includes address of the recipient, the amount of ETH to be staked and the transaction type: 
```     
    *1* for staking Eth to get frxEth.
    *2* for staking Eth and then staking frxEth to get sFrxEth(staked Frax ETH).
```

```javascript
    (         
            address _recipient, 
            uint256 _amount,
            uint256 _txType
    )
```

- **Step 2:** Receives the respective funds.

- **Step 3:** Calls the *_stake* function.

**3. "_stake" Function:** This internal function is responsible for calling the Frax, staking the funds on Frax and transferring the Frax asset to the user/recipient.
