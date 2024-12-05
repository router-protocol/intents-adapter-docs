# Deposit Liquidity and Mint A Position on Maverick

The Maverick Deposit adapter outlines a feature that enables users holding funds on any compatible chain to add liquidity on Maverick in one step.

## Components of the Maverick Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token along with the address of Maverick Router for the respective chain. (The Maverick Router allows routes through any pools created by Maverick factory. The adapter mainly interacts with this contract).

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data is in the form of a struct that includes token addresses for which liquidity is to be added, respective amounts of tokens desired to be added, address of the recipient and data for the transaction.

```javascript
    struct MaverickSupplyData {
        address tokenA;
        address tokenB;
        uint256 tokenAAmount;
        uint256 tokenBAmount;
        address recipient;
        bytes[] data;
    }
```

- **Step 2:** Receives the respective tokens.

- **Step 3:** Calls the _mint_ function.

**3. "mint" Function:** This function is responsible for calling the Maverick Router, minting the position on Maverick and transferring the LP asset/recipet tokens to the user/recipient.
