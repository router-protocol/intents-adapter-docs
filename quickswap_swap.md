# Use Quickswap Swap Adapter for Swaps on XLayer

The Quickswap Swap adapter outlines a feature that enables users holding funds on any compatible chain to swap for tokens desired on XLayer chain.  

## Components of the Quick Swap Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token along with the address of their Swap Router for the respective chain.

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data is in the form of a struct that includes the address of the given token, address of the desired token, address of the recipient, deadline for the transaction, amount, minimum amount to be received and limitSqrtPrice.

```javascript
    struct ExactInputSingleParams {
        address tokenIn;
        address tokenOut;
        address recipient;
        uint256 deadline;
        uint256 amountIn;
        uint256 amountOutMinimum;
        uint160 limitSqrtPrice;
    }
```

- **Step 2:** Receives the respective tokens.

- **Step 3:** Calls the *_swap* function.

**3. "_swap" Function:** This function is responsible for calling the Quickswap Swap Router, swapping the tokens and transferring the swapped asset to the user/recipient.

