# Deposit Liquidity on Lynex Gamma

The Lynex Gamma adapter outlines a feature that enables users holding funds on any compatible chain to add liquidity on Lynex Gamma in one step.  

## Components of the Lynex Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token along with the address of Lynex Gamma entry point and Lynex Clearing contract for the respective chain.

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data is in the form of a struct that includes token addresses for which liquidity is to be added, respective amounts of tokens desired to be added, address of the recipient, address of the pool for tokens and minimum amounts of tokens to be added.

```javascript
    struct LynexDepositData {
        address tokenA;
        address tokenB;
        uint256 depositA;
        uint256 depositB;
        address to;
        address pos;
        uint256[4] minIn;
    }
```

- **Step 2:** Receives the respective tokens.

- **Step 3:** Calls the *_mint* function.

**3. "_mint" Function:** This function is responsible for calling the Lynex Gamma, minting the position on Lynex Gamma and transferring the LP asset/recipet tokens to the user/recipient.
