# Stake BNB on Synclub

The Synclub Stake BNB adapter outlines a feature that enables users holding funds on any compatible chain to stake BNB on Synclub in one step.

## Components of the Synclub Stake BNB Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, sn-BNB Synclub token and the address of the Synclub Pool for the respective chain.

```solidity
    constructor(
        address __native,
        address __wnative,
        address __snBnb,
        address __synClubPool
    )
```

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data includes address of the recipient and the amount of BNB to be staked.

```javascript
    (         
            address _recipient, 
            uint256 _amount
    )
```

- **Step 2:** Receives the respective funds.

- **Step 3:** Calls the *_stake* function.

**3. "_stake" Function:** This internal function is responsible for calling the Synclub, staking the funds on Synclub and transferring the Synclub sn-BNB to the user/recipient.
