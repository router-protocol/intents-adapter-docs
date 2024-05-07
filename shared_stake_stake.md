# Stake ETH on SharedStake

The SharedStake Stake ETH adapter outlines a feature that enables users holding funds on any compatible chain to stake ETH on SharedStake in one step.

## Components of the SharedStake Stake ETH Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, wsg-ETH SharedStake token and the address of the Deposit Minter for the respective chain.

```solidity
    constructor(
        address __native,
        address __wnative,
        address __wsgEth,
        address __depositMinter
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

**3. "_stake" Function:** This internal function is responsible for calling the SharedStake, staking the funds on SharedStake and transferring the SharedStake wsg-ETH to the user/recipient.
