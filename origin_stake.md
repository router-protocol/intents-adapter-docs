# Stake ETH on Origin

The Origin Stake ETH adapter outlines a feature that enables users holding funds on any compatible chain to stake ETH on Origin in one step.

## Components of the Origin Stake ETH Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, o-ETH Origin token and the address of the Origin o-ETH Zapper for the respective chain.

```solidity
    constructor(
        address __native,
        address __wnative,
        address __oETH,
        address __oETHZapper
    )
```

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data includes address of the recipient and the amount of ETH to be staked.

```javascript
    (         
            address _recipient, 
            uint256 _amount
    )
```

- **Step 2:** Receives the respective funds.

- **Step 3:** Calls the *_stake* function.

**3. "_stake" Function:** This internal function is responsible for calling the Origin, staking the funds on Origin and transferring the Origin o-ETH to the user/recipient.
