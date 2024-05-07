# Supply on LAYER BANK

The Layer Bank Supply adapter outlines a feature that enables users holding funds on any compatible chain to supply funds on Layer Bank in one step.  

## Components of the Layer Bank Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, Layer Bank Core Contract, USDC, wBTC and wstETH for the respective chain.

```javascript
    // For Linea
    constructor(
        address __native,
        address __wnative,
        address __layerBankCore,
        address __usdc,
        address __wBtc,
        address __wstEth
    )

    // For Scroll
    constructor(
        address __native,
        address __wnative,
        address __layerBankCore,
        address __usdc,
        address __wstEth
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

- **Step 3:** Calls the *_layerBankSupply* function.

**3. "_layerBankSupply" Function:** This function is responsible for calling the Layer Bank Core Contract, supplying the asset on Layer Bank and transferring the reciept asset to the user/recipient.
