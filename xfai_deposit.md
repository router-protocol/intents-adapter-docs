# Deposit Liquidity and Mint A Position on Xfai

The Xfai Deposit adapter outlines a feature that enables users holding funds on any compatible chain to add liquidity on Xfai in one step. (One of the two tokens has to be ETH) 

## Components of the Xfai Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token along with the address of Xfai Periphery contract for the respective chain. (The adapter mainly interacts with this contract).

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data is in the form of a struct that includes address of the recipient, token address other than that of ETH for which liquidity is to be added, respective amount of token desired to be added, amount of ETH desired to be added, minimum amounts of tokens to be added and deadline for the transaction. Note that user has to send msg value for depositing liquidity for ETH.

```javascript
    struct XfaiSupplyData {
        address _to;
        address _token;
        uint _amountTokenDesired;
        uint _amountETHDesired;
        uint _amountTokenMin;
        uint _amountETHMin;
        uint _deadline;
    }
```

- **Step 2:** Receives the respective tokens.

- **Step 3:** Calls the *_mint* function.

**3. "_mint" Function:** This function is responsible for calling the Xfai Periphery, minting the position on Xfai and transferring the LP asset/reciept tokens if any to the user/recipient.