# Stake AVAX on Benqi

The Benqi Stake AVAX adapter outlines a feature that enables users holding funds on any compatible chain to stake AVAX on Benqi (on Avalanche) in one step.

## Components of the Benqi Stake AVAX Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, and the address of the Benqi s-AVAX token for the respective chain.

```solidity
    constructor(
        address __native,
        address __wnative,
        address __benqiSAvax
    )
```

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data includes address of the recipient and the amount of AVAX to be staked.

```javascript
    (         
            address _recipient, 
            uint256 _amount
    )
```

- **Step 2:** Receives the respective funds.

- **Step 3:** Calls the *_stake* function.

**3. "_stake" Function:** This internal function is responsible for calling the Benqi, staking the funds on Benqi and transferring the Benqi s-AVAX to the user/recipient.
