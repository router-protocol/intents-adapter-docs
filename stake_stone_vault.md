# Stake WETH on StakeStone on XLayer

The StakeStoneVault Stake WETH adapter outlines a feature that enables users holding funds on any compatible chain to stake funds on StakeStone (on XLayer) in one step

## Components of the StakeStoneVault Stake WETH Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, weth token, stone token and the address of the stone vault deployed by Router on XLayer for the respective chain.

```solidity
    constructor(
        address __native,
        address __wnative,
        address __weth,
        address __stone,
        address __stakeStoneVault
    )
```

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data includes address of the recipient and the amount of WETH to be staked.

```javascript
    (         
            address _recipient, 
            uint256 _amount
    )
```

- **Step 2:** Receives the respective funds.

- **Step 3:** Calls the *_stake* function.

**3. "_stake" Function:** This internal function is responsible for calling the StakeStone Vault on XLayer, staking the funds on that vault and transferring the StakeStone asset to the user/recipient.
