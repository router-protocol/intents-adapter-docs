# Deposit Liquidity and Mint A Position on Velocore

The Velocore Deposit adapter outlines a feature that enables users holding funds on any compatible chain to add liquidity on Velocore in one step.  

## Components of the Velocore Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, address of the Velocore token along with the address of Velocore Vault for the respective chain. (The Velocore Vault is a singleton contract storing user-deposited tokens. It stores balances of pools and depositors. The adapter mainly interacts with this contract).

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data is in the form of a struct that includes token addresses for which liquidity is to be added, address of the LP token, address of the recipient and respective amounts of token desired to be added.

```javascript
    struct VelocoreSupplyData {
        address tokenA;
        address tokenB;
        address lpToken;
        address to;
        uint256 amountADesired;
        uint256 amountBDesired;
    }
```

- **Step 2:** Receives the respective tokens.

- **Step 3:** Calls the *_mint* function.

**3. "_mint" Function:** This function is responsible for calling the Velocore Vault, minting the position on Velocore and transferring the LP asset to the user/recipient.
