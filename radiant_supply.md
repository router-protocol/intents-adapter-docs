# Supply on RADIANT

The Radiant Supply adapter outlines a feature that enables users holding funds on any compatible chain to supply funds on Radiant in one step.  

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

- **Step 1:** Decodes the data received. Here the data includes the address of the asset to be supplied, address of the recipient, and the amount of tokens to be supplied.

```javascript
    (
        address _asset,
        address _recipient,
        uint256 _amount
    )
```

- **Step 2:** Receives the respective tokens.

- **Step 3:** Calls the *_radiantSupply* function.

**3. "_radiantSupply" Function:** This function is responsible for calling the Radiant Pool or Radiant Token Gateway, supplying the asset on Radiant and transferring the reciept asset to the user/recipient.
