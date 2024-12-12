# Supply on COMPOUND

The Compound Supply adapter outlines a feature that enables users holding funds on any compatible chain to supply funds on Compound in one step.  

## Components of the Compound Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, USDC, Compound USDC Pool and Compound WETH Pool.

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

- **Step 1:** Decodes the data received. Here the data includes the address of the asset to be supplied, address of the recipient, the amount of tokens to be supplied and the address of asset(market) where user wants to supply to.

```javascript
    (
        address _asset,
        address _recipient,
        uint256 _amount,
        address _market
    )
```

- **Step 2:** Receives the respective tokens.

- **Step 3:** Calls the *_compoundSupply* function.

**3. "_compoundSupply" Function:** This function is responsible for calling the respective Compound Market, supplying the asset on Compound and transferring the reciept asset to the user/recipient.
