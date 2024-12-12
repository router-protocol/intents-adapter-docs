# Stake ETH on Dinero

The Dinero Stake ETH adapter outlines a feature that enables users holding funds on any compatible chain to stake ETH on Dinero in one step.

## Components of the Dinero Stake ETH Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, px-ETH Dinero token and the address of the Pirex Pool for the respective chain.

```solidity
    constructor(
        address __native,
        address __wnative,
        address __pxEth,
        address __pirexPool
    )
```

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data includes address of the recipient, the amount of ETH to be staked and the boolean value whether to also compound on pirex vault (should put `false` if only staking intended).

```javascript
    (         
            address _recipient, 
            uint256 _amount,
            bool _shouldCompound
    )
```

- **Step 2:** Receives the respective funds.

- **Step 3:** Calls the *_stake* function.

**3. "_stake" Function:** This internal function is responsible for calling the Dinero, staking the funds on Dinero and transferring the Dinero px-ETH to the user/recipient.
