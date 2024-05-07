# Borrow on AAVE V3

The Aave Borrow adapter facilitates cross-chain borrowing after the user delegates their borrowing power to the adapter. Once delegated, the adapter manages the borrowing process from Aave on behalf of the user, delivering funds wherever they're needed.

## Components of the Aave Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, Aave Pool, Aave Wrapped Token gateway along with the referral code for the respective chain.

```javascript
    constructor(
        address __native,
        address __wnative,
        address __aaveV3Pool,
        address __aaveV3WrappedTokenGateway,
        uint16 __aaveV3ReferralCode
    )
```

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data includes the amount of asset to be borrowed, the rate mode for the borrowing (`1` for stable rate, `2` for variable rate), the address of the asset to be borrowed, address on whose behalf the borrowing is to be incurred and the address of the recipient of the funds.

```javascript
    (
        uint256 amount,
        uint256 rateMode,
        address asset,
        address onBehalfOf,
        address recipient
    )
```

- **Step 2:** Calls the *_aaveV3Borrow* function.

**3. "_aaveV3Borrow" Function:** This function is responsible for calling the Aave Pool , borrowing the asset on Aave and transferring the borrowed asset to the user/recipient. Note that if the users want to bridge these borrowed funds on other chain, they have to use Nitro adapter for it.
