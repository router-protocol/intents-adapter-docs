# Deposit Liquidity and Mint A Position on Izumi

The Izumi Deposit adapter outlines a feature that enables users holding funds on any compatible chain to add liquidity and mint a new position on Izumi in one step.  

## Components of the Izumi Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token along with the address of Liquidity manager for the respective chain.

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data is in the form of a struct that includes address of the recipient,  token addresses for which liquidity is to be added, respective fee, ticks associated with the txn (can be fetched from their sdk), respective amounts of token desired to be added, minimum amounts of tokens to be added and deadline for the transaction.

```javascript
    struct MintParams {
        // miner address
        address miner;
        // tokenX of swap pool
        address tokenX;
        // tokenY of swap pool
        address tokenY;
        // fee amount of swap pool
        uint24 fee;
        // left point of added liquidity
        int24 pl;
        // right point of added liquidity
        int24 pr;
        // amount limit of tokenX miner willing to deposit
        uint128 xLim;
        // amount limit tokenY miner willing to deposit
        uint128 yLim;
        // minimum amount of tokenX miner willing to deposit
        uint128 amountXMin;
        // minimum amount of tokenY miner willing to deposit
        uint128 amountYMin;
        uint256 deadline;
    }
```

- **Step 2:** Receives the respective tokens.

- **Step 3:** Calls the *_mint* function.

**3. "_mint" Function:** This function is responsible for calling the Liquidity manager, minting the position on Izumi and transferring the LP asset to the user/recipient.
