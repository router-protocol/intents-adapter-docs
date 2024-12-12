# Deposit Liquidity and Mint A Position on Pendle

The Pendle Deposit adapter outlines a feature that enables users holding funds on any compatible chain to add liquidity on Pendle in one step.  

## Components of the Pendle Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token along with the address of Pendle Router for the respective chain. (The adapter mainly interacts with Pendle Router contract).

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data is in the form of a struct that includes token addresses for which liquidity is to be added, address of the LP token, address of the recipient and respective amounts of token desired to be added.

```javascript
    struct PendleSupplyData {
        address receiver;
        address market;
        uint256 minLpOut;
        ApproxParams guessPtReceivedFromSy;
        TokenInput input;
        LimitOrderData limit;
    }
```
User can read about structs mentioned in above code snippet such as *ApproxParams*, *TokenInput*, *LimitOrderData* [here](https://docs.pendle.finance/Developers/Contracts/PendleRouter#important-structs-in-pendlerouter).

- **Step 2:** Receives the respective tokens.

- **Step 3:** Calls the *_mint* function.

**3. "_mint" Function:** This function is responsible for calling the Pendle Router, minting the position on Pendle and transferring the LP asset to the user/recipient.
