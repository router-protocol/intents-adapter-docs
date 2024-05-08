# Supply on BENQI

The Benqi Supply adapter outlines a feature that enables users holding funds on any compatible chain to supply funds on Benqi in one step.  

## Components of the Benqi Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token and the respective Qi Token associated with market to be supplied to, for the respective chain.

```javascript
    constructor(
        address __native,
        address __wnative,
        address __qiToken
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

- **Step 3:** Calls the *_benqiSupply* function.

**3. "_benqiSupply" Function:** This function is responsible for calling the Benqi Token Pool, supplying the asset on Benqi and transferring the reciept asset to the user/recipient.
