# Stake USDC/WETH on PARIFI

The Parifi Vault Deposit adapter outlines a feature that enables users holding funds on any compatible chain to stake USDC/WETH on Parifi in one step.

## Components of the Parifi Vault Deposit Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, address of the USDC token, address of pf-usdc recipet token along with the address of pf-weth token for the respective chain.

```solidity
    constructor(
        address __native,
        address __wnative,
        address __usdc,
        address __pfUsdc,
        address __pfWeth
    )
```

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data includes the address of the asset to be staked (USDC/WETH), the address of the recipient and amount of the asset to be staked, 

```javascript
    (         
            address _asset,
            address _recipient, 
            uint256 _amount
    )
```

- **Step 2:** Receives the respective tokens.

- **Step 3:** Calls the *_stake* function.

**3. "_stake" Function:** This internal function is responsible for calling the Parifi PF Token, staking the funds on Parifi and transferring the PF (pf-USDC, pf-WETH) receipt asset to the user/recipient.
