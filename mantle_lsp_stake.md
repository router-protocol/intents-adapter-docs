# Stake ETH on Mantle-LSP

The Mantle-LSP Stake ETH adapter outlines a feature that enables users holding funds on any compatible chain to stake ETH on Mantle-LSP in one step.

## Components of the Mantle-LSP Stake ETH Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, m-ETH mantle token and the address of the Mantle-LSP Pool for the respective chain.

```solidity
    constructor(
        address __native,
        address __wnative,
        address __mEth,
        address __mantlePool
    )
```

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data includes address of the recipient, the amount of ETH to be staked and the minimum amount of M-ETH to be received.

```javascript
    (         
            address _recipient, 
            uint256 _amount,
            uint256 minMETHAmount
    )
```

- **Step 2:** Receives the respective funds.

- **Step 3:** Calls the *_stake* function.

**3. "_stake" Function:** This internal function is responsible for calling the Mantle-LSP, staking the funds on Mantle-LSP and transferring the Mantle-LSP m-ETH to the user/recipient.
