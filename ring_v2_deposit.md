# Deposit Liquidity and Mint A Position on Ring

The Ring Deposit adapter outlines a feature that enables users holding funds on any compatible chain to add liquidity on Ring in one step.  

## Components of the Ring Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token along with the address of Ring Router for the respective chain. (The Ring Router allows routes through any pools created by Ring factory. The adapter mainly interacts with this contract).

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data is in the form of a struct that includes token addresses for which liquidity is to be added, respective amounts of tokens desired to be added, minimum amounts of tokens to be added, address of the recipient and deadline for the transaction.

```javascript
    struct RingSupplyData {
        address tokenA;
        address tokenB;
        uint amountADesired;
        uint amountBDesired;
        uint amountAMin;
        uint amountBMin;
        address to;
        uint deadline;
    }
```

- **Step 2:** Receives the respective tokens.

- **Step 3:** Calls the *_mint* function.

**3. "_mint" Function:** This function is responsible for calling the Ring Router, minting the position on Ring and transferring the LP asset/recipet tokens to the user/recipient.
