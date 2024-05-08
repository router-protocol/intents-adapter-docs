# Borrow on Compound

The Compound Borrow adapter facilitates cross-chain borrowing after the user delegates their borrowing power to the adapter. Once delegated, the adapter manages the borrowing process from Compound on behalf of the user, delivering funds wherever they're needed.

## Components of the Compound Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, USDC, cUSDC market and the cWETH market for the respective chain.

```javascript
    constructor(
        address __native,
        address __wnative,
        address __usdc,
        address __cUSDCV3Pool,
        address __cWETHV3Pool
    )
```

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data includes the amount of asset to be borrowed, the address of the asset to be borrowed, address on whose behalf the borrowing is to be incurred and the address of the recipient of the funds.

```javascript
    (
        uint256 amount,
        address asset,
        address onBehalfOf,
        address recipient
    )
```

- **Step 2:** Calls the *_compoundBorrow* function.

**3. "_compoundBorrow" Function:** This function is responsible for calling the respective Compound Market , borrowing the asset on Compound and transferring the borrowed asset to the user/recipient. Note that if the users want to bridge these borrowed funds on other chain, they have to use Nitro adapter for it.
