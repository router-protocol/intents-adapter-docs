# Open a position on PARIFI (PERPETUAL PROTOCOL)

The Parifi Open Position adapter outlines a feature that enables users holding funds on any compatible chain to open a new position on Parifi in one step.  

## Components of the Camelot Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, Parify Order Manager along with the address of Parifi Data Fabric for the respective chain.

```javascript
    constructor(
            address __native,
            address __wnative,
            address __parifiOrderManager,
            address __parifiDataFabric
        )
```

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data is in the form of a struct that includes the following parameters:

```javascript
    struct Order {
        bytes32 marketId; // keccak256 hash of asset symbol + vaultAddress
        address userAddress; // User that signed/submitted the order
        OrderType orderType; // Refer enum OrderType
        bool isLong; // Set to true if it is a Long order, false for a Short order
        bool isLimitOrder; // Flag to identify limit orders
        bool triggerAbove; // Flag to trigger price above or below expectedPrice
        uint256 deadline; // Timestamp after which order cannot be executed
        uint256 deltaCollateral; // Change in collateral amount (increased/decreased)
        uint256 deltaSize; // Change in Order size (increased/decreased)
        uint256 expectedPrice; // Desired Value for order execution
        uint256 maxSlippage; // Maximum allowed slippage in executionPrice from expectedPrice (in basis points)
        address partnerAddress; // Address that receives referral fees for new position orders (a share of opening fee)
    }
```

- **Step 2:** Receives the respective tokens.

- **Step 3:** Calls the *_openNewPosition* function.

**3. "_openNewPosition" Function:** This function is responsible for calling the Parify Order Manager and opening a position on Parifi for the user that submitted the order.