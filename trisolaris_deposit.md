# Deposit Liquidity and Mint A Position on Trisolaris

The Trisolaris Deposit adapter outlines a feature that enables users holding funds on any compatible chain to add liquidity on Trisolaris in one step.  

## Components of the Trisolaris Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token along with the address of Trisolaris Router for the respective chain. (The Trisolaris Router allows routes through any pools created by Trisolaris factory. The adapter mainly interacts with this contract).

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data is in the form of a struct that includes token addresses for which liquidity is to be added, address of the recipient, respective amounts of token desired to be added, minimum amounts of tokens to be added and deadline for the transaction.

```javascript
    struct TrisolarisSupplyData {
        address tokenA;
        address tokenB;
        address to;
        uint256 amountADesired;
        uint256 amountBDesired;
        uint256 amountAMin;
        uint256 amountBMin;
        uint256 deadline;
    }
```

- **Step 2:** Receives the respective tokens.

- **Step 3:** Calls the *_mint* function.

**3. "_mint" Function:** This function is responsible for calling the Trisolaris Router, minting the position on Trisolaris and transferring the LP asset/recipet tokens to the user/recipient.
