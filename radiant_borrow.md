# Borrow on RADIANT

The Radiant Borrow adapter facilitates cross-chain borrowing after the user delegates their borrowing power to the adapter. Once delegated, the adapter manages the borrowing process from Radiant on behalf of the user, delivering funds wherever they're needed.

## Components of the Radiant Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, Radiant Pool, Radiant Wrapped Token gateway along with the referral code for the respective chain.

```javascript
    constructor(
        address __native,
        address __wnative,
        address __radiantPool,
        address __radiantWrappedTokenGateway,
        uint16 __radiantReferralCode
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

- **Step 2:** Calls the *_radiantBorrow* function.

**3. "_radiantBorrow" Function:** This function is responsible for calling the Radiant Pool , borrowing the asset on Radiant and transferring the borrowed asset to the user/recipient. Note that if the users want to bridge these borrowed funds on other chain, they have to use Nitro adapter for it.
