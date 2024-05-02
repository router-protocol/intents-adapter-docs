# Deposit Liquidity and Mint A Position on Aerodrome

The Aerodrome Deposit adapter outlines a feature that enables users holding funds on any compatible chain to add liquidity on Aerodrome in one step.  

## Components of the Aerodrome Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token along with the address of Aerodrome Router for the respective chain. (The Aerodrome Router allows routes through any pools created by Aerodrome factory. The adapter mainly interacts with this contract).

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data is in the form of a struct that includes token addresses for which liquidity is to be added, boolean value for if the pool is stable, respective amounts of token desired to be added, minimum amounts of tokens to be added, address of the recipient and deadline for the transaction.

```javascript
    struct AeroSupplyData {
        address tokenA;
        address tokenB;
        bool stable;
        uint256 amountADesired;
        uint256 amountBDesired;
        uint256 amountAMin;
        uint256 amountBMin;
        address to;
        uint256 deadline;
    }
```

- **Step 2:** Receives the respective tokens.

- **Step 3:** Calls the *_mint* function.

**3. "_mint" Function:** This function is responsible for calling the Aerodrome Router, minting the position on Aerodrome and transferring the LP asset/recipet tokens to the user/recipient.
