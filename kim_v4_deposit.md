# Deposit Liquidity and Mint A Position on Kim V4

The Kim Deposit adapter outlines a feature that enables users holding funds on any compatible chain to add liquidity and mint a new position on Kim in one step.  

## Components of the Kim Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token along with the address of Non-fungible position manager for the respective chain.

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data is in the form of a struct that includes token addresses for which liquidity is to be added, ticks associated with the txn, respective amounts of token desired to be added, minimum amounts of tokens to be added , address of the recipient and deadline for the transaction.

```javascript
    struct MintParams {
        address token0;
        address token1;
        int24 tickLower;
        int24 tickUpper;
        uint256 amount0Desired;
        uint256 amount1Desired;
        uint256 amount0Min;
        uint256 amount1Min;
        address recipient;
        uint256 deadline;
    }
```

- **Step 2:** Receives the respective tokens.

- **Step 3:** Calls the *_mint* function.

**3. "_mint" Function:** This function is responsible for calling the Non-fungible position manager, minting the position on Kim and transferring the LP asset to the user/recipient.
