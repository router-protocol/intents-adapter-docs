# Stake on Ankr

The ANKR Stake adapters outline a feature that enables users holding funds on any compatible chain to stake tokens on Ankr in one step.

## Components of the Ankr Stake Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, ankrToken (eg: AnkrEth, AnkrMatic, AnkrBsc etc) and the address of the Ankr Pool for the respective chain.

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

**3. "_stake" Function:** This internal function is responsible for calling the Ankr, staking the funds on Ankr and transferring the Ankr asset to the user/recipient.
