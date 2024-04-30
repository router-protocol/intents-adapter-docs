# Use the Nitro Dexspan for the Most Efficient Swap

The Dexspan adapter outlines a feature that enables users to interact with the Nitro Dexspan contract for the most efficient swaps.  

## Components of the Dexspan Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token and the address of the dexspan for the respective chain. 

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data is in the form of a struct that includes path of the token swap, amount, minimum amount to be received, flags , swap data and address of the recipient.

```javascript
    struct SameChainSwapParams {
        IERC20[] tokens;
        uint256 amount;
        uint256 minReturn;
        uint256[] flags;
        bytes[] dataTx;
        address recipient;
    }
```

- **Step 2:** Receives the respective tokens.

- **Step 3:** Calls the *_swap* function.

**3. "_swap" Function:** This function is responsible for calling the Dexspan, swapping the tokens and transferring the swapped asset to the user/recipient.
