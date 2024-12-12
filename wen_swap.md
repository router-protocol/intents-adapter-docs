# Use WenSwapper Adapter for Swap on Polygon (meant for Wen Tokens)

The Wen Swap adapter outlines a feature that enables users holding funds on any compatible chain to swap to or from Wen tokens desired on Polygon chain.     

## Components of the Wen Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token and the address of the Wen Foundry for the respective chain. 

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller (batch transaction contract).

- **Step 1:** Decodes the data received. Here the data is in the form of a struct that includes the address of the given token, address of the desired token, amount, minimum amount to be received, address of the recipient, deadline for the transaction and the transaction type ( txType *1* - swap ETH for WenTokens , txType *2* - swap WenTokens for ETH).

```javascript
    struct WenSwapParams {
        address tokenIn;
        address tokenOut;
        uint256 amountIn;
        uint256 amountOutMin;
        address to;
        uint256 deadline;
        uint8 txType;
    }
```

- **Step 2:** Receives the respective tokens.

- **Step 3:** Calls the *_swap* function.

**3. "_swap" Function:** This function is responsible for calling the Wen foundry, swapping the tokens and transferring the swapped asset to the user/recipient.
