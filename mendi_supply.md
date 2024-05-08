# Supply on MENDI

The Mendi Supply adapter outlines a feature that enables users holding funds on any compatible chain to supply funds on Mendi in one step.  

## Components of the Mendi Adapter contract

**1. Constructor:** The constructor takes the following parameters for the respective chain:

```javascript
    constructor(
        address __native,
        address __wnative,
        address __usdc,
        address __usdt,
        address __dai,
        address __wbtc,
        address __wstEth,
        address __meWeth,
        address __meUsdc,
        address __meUsdt,
        address __meDai,
        address __meWbtc,
        address __meWstEth
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

- **Step 3:** Calls the *_mendiSupply* function.

**3. "_mendiSupply" Function:** This function is responsible for calling the Mendi Pool or Mendi Token Gateway, supplying the asset on Mendi and transferring the reciept asset to the user/recipient.
