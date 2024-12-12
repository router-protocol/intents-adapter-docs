# Stake on Stader

The Stader Stake adapters outline a feature that enables users holding funds on any compatible chain to stake tokens on Stader in one step.

## Components of the Stader Stake Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, staderToken (eg: ethX, maticX, bnbX etc) and the address of the Stader Pool for the respective chain.

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data includes address of the recipient and the amount of tokens to be staked.

```javascript
    (         
            address _recipient, 
            uint256 _amount
    )
```

- **Step 2:** Receives the respective funds.

- **Step 3:** Calls the *_stake* function.

**3. "_stake" Function:** This internal function is responsible for calling the Stader, staking the funds on Stader and transferring the Stader asset to the user/recipient.
