# Stake wETH, weETH, eETH on ETHER-FI

The EtherFi Stake adapter outlines a feature that enables users holding funds on any compatible chain to stake wETH, weETH or eETH on EtherFi in one step.

## Components of the EtherFi Stake Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token along with the address of EtherFi Liquid Vault for the respective chain.

```solidity
    constructor(
        address __native,
        address __wnative,
        address __liquidVault
    ) 
```

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data includes address of the asset to be staked(wETH, weETH or eETH), recipient and amount of tokens to be staked, 

```javascript
    (         
            address _asset,
            address _recipient, 
            uint256 _amount
    )
```

- **Step 2:** Receives the respective tokens.

- **Step 3:** Calls the *_stake* function.

**3. "_stake" Function:** This internal function is responsible for calling the EtherFi Liquid Vault, staking the funds on EtherFi and transferring the EtherFi receipt asset to the user/recipient.
