# Guide: Creating an Intent Adapter with Router Intents

This guide will walk you through creating a general intent adapter using Router Protocol's package. We will cover the steps from setting up dependencies, creating the contract, defining functions, and calling them.

## Prerequisites

Ensure you have the following set up before proceeding:

- **Node.js** and **npm**
- **Hardhat**
- Basic understanding of **Solidity**

## Step 1: Set Up the Project

1. **Initialize the project directory:**

   Open your terminal and create a project directory:

   ```bash
   mkdir my-intent-adapter
   cd my-intent-adapter
   ```

2. **Initialize Node.js project:**

   ```bash
   npm init -y
   ```

3. **Install Hardhat:**

   ```bash
   npm install --save-dev hardhat
   ```

4. **Create a new Hardhat project:**
   ```bash
   npx hardhat
   ```

## Step 2: Install Required Dependencies

1. **Install Router Protocol Intents packages:**

   ```bash
   npm install @routerprotocol/intents-core
   ```

   or

   ```bash
   yarn add @routerprotocol/intents-core
   ```

2. **Install other dependencies:** Install the dependencies needed for SafeERC20.
   ```bash
   npm install @openzeppelin/contracts
   ```
   or
   ```bash
   yarn add @openzeppelin/contracts
   ```

## Step 3: Define the Adapter Contract

Create a Solidity file for your adapter in the `contracts` folder.

Example: StaderStakeEth Adapter: This adapter uses router intents framework to enable users that have funds on any compatible chain to stake their funds on Stader on Ethereum. (Modify for your own protocol)

_INTERFACE_

```javascript
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.18;

    interface IStaderPool {
        function deposit(address receiver) external payable returns (uint256);

        function swapMaticForMaticXViaInstantPool() external payable;
    }
```

_ADAPTER CONTRACT_

```javascript
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.18;

    import {IStaderPool} from "./Interfaces.sol";
    import {RouterIntentEoaAdapterWithoutDataProvider, EoaExecutorWithoutDataProvider} from "@routerprotocol/intents-core/contracts/RouterIntentEoaAdapter.sol";
    import {Errors} from "@routerprotocol/intents-core/contracts/utils/Errors.sol";
    import {IERC20, SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

    /**
     * @title StaderStakeEth
     * @notice Example adapter for staking ETH on Stader protocol.
     */
    contract StaderStakeEth is RouterIntentEoaAdapterWithoutDataProvider {
        using SafeERC20 for IERC20;

        address public immutable ethx;
        IStaderPool public immutable staderPool;

        constructor(
            address __native,
            address __wnative,
            address __ethx,
            address __staderPool
        ) RouterIntentEoaAdapterWithoutDataProvider(__native, __wnative) {
            ethx = __ethx;
            staderPool = IStaderPool(__staderPool);
        }

        function name() public pure override returns (string memory) {
            return "StaderStakeEth";
        }

        function execute(
            bytes calldata data
        ) external payable override returns (address[] memory tokens) {
            (address _recipient, uint256 _amount) = parseInputs(data);

            if (address(this) == self()) {
                require(
                    msg.value == _amount,
                    Errors.INSUFFICIENT_NATIVE_FUNDS_PASSED
                );
            } else if (_amount == type(uint256).max)
                _amount = address(this).balance;

            bytes memory logData;
            (tokens, logData) = _stake(_recipient, _amount);

            emit ExecutionEvent(name(), logData);
            return tokens;
        }

        function _stake(
            address _recipient,
            uint256 _amount
        ) internal returns (address[] memory tokens, bytes memory logData) {
            staderPool.deposit{value: _amount}(_recipient);

            tokens = new address ;
            tokens[0] = native();
            tokens[1] = ethx;

            logData = abi.encode(_recipient, _amount);
        }

        function parseInputs(
            bytes memory data
        ) public pure returns (address, uint256) {
            return abi.decode(data, (address, uint256));
        }

        receive() external payable {}
    }

```

- IMPORTS:

  1. `RouterIntentEoaAdapterWithoutDataProvider` and `EoaExecutorWithoutDataProvider` from the Router Protocol Intents package.

  2. `IERC20` and `SafeERC20` for token transfers.

- CONSTRUCTOR:
  The constructor accepts addresses for native token, wnative token and protocol-specific contracts or the contracts containing entry points for the protocol. (e.g., ethx, staderPool).

- FUNCTIONS:

  1. `execute`: This function has to be present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract). It parses the input, ensures the correct value is passed, and executes the \_stake function.

  2. `_stake`: This is an internal function that interacts with the protocol (in this case, the StaderPool) to deposit funds.

## Step 4: Customize for Your Protocol

To adapt the contract to your own protocol, follow these steps:

**Replace Protocol-Specific Logic:** 1. Change the external contract interfaces (e.g., `IStaderPool`) to match your protocol's interface. 2. Change the constructor arguments according to the protocol. (the address of `native` and `wnative` tokens at first two places remains fixed). 3. Adjust the parsing according to the needed arguments in the `execute` function and `parseInputs` function. 4. Modify the name of the internal function and logic in `_stake` to interact with protocol’s staking, lending, or swapping functions. 5. Adjust the ERC20 token interactions as necessary (use `SafeERC20` for safety).

In this step, we explain how to customize the provided PancakeswapMint adapter for PancakeSwap or any other protocol. The goal is to replace protocol-specific logic, imports, and interfaces with the equivalent components of the new protocol you're targeting.

**Example: Adapting for a new Protocol (PancakeSwap Mint)**

1. In `PancakeswapMint` adapter, the contract interacts with the `IPancakeswapNonfungiblePositionManager` interface for minting positions.

```javascript
import { IPancakeswapNonfungiblePositionManager } from "./Interfaces.sol";
```

2. The core function is `_mint`, which handles the logic for minting a new position on PancakeSwap. This function interacts with the PancakeSwap contract to mint a position by calling `nonFungiblePositionManager.mint(mintParams)`.

```javascript
    function _mint(
        IPancakeswapNonfungiblePositionManager.MintParams memory mintParams
    ) internal returns (address[] memory tokens, bytes memory logData) {
        (uint256 tokenId, , , ) = nonFungiblePositionManager.mint(mintParams);

        tokens = new address ;
        tokens[0] = mintParams.token0;
        tokens[1] = mintParams.token1;

        logData = abi.encode(mintParams, tokenId);
    }

```

3. The `execute` function handles transferring tokens to the contract and managing approvals before minting a position:

```javascript
IERC20(mintParams.token0).safeIncreaseAllowance(
  address(nonFungiblePositionManager),
  mintParams.amount0Desired
);
```

4. The `parseInputs` function decodes the `MintParams` for the PancakeSwap position manager.

```javascript
    function parseInputs(
        bytes memory data
    ) public pure returns (IPancakeswapNonfungiblePositionManager.MintParams memory) {
        return abi.decode(data, (IPancakeswapNonfungiblePositionManager.MintParams));
    }
```

- For another protocol, you need to replace this with the appropriate input structure that matches the action being performed.

5. Ensure that the constructor is updated to reflect the new protocol's requirements. In the provided `PancakeswapMint` contract, the constructor accepts the address for `nonFungiblePositionManager`:

```javascript
    constructor(
    address __native,
    address __wnative,
    address __nonFungiblePositionManager
    )
```
- Also, update the deployment script to pass the correct contract addresses during deployment.

*INTERFACE FOR PANCAKESWAP MINT*
```javascript
// SPDX-License-Identifier: GPL-2.0-or-later
pragma solidity ^0.8.18;

abstract contract IPancakeswapNonfungiblePositionManager {
    struct MintParams {
        address token0;
        address token1;
        uint24 fee;
        int24 tickLower;
        int24 tickUpper;
        uint256 amount0Desired;
        uint256 amount1Desired;
        uint256 amount0Min;
        uint256 amount1Min;
        address recipient;
        uint256 deadline;
    }

    function mint(
        MintParams calldata params
    )
        external
        payable
        virtual
        returns (
            uint256 tokenId,
            uint128 liquidity,
            uint256 amount0,
            uint256 amount1
        );

    struct IncreaseLiquidityParams {
        uint256 tokenId;
        uint256 amount0Desired;
        uint256 amount1Desired;
        uint256 amount0Min;
        uint256 amount1Min;
        uint256 deadline;
    }

    function increaseLiquidity(
        IncreaseLiquidityParams calldata params
    )
        external
        payable
        virtual
        returns (uint128 liquidity, uint256 amount0, uint256 amount1);

    struct DecreaseLiquidityParams {
        uint256 tokenId;
        uint128 liquidity;
        uint256 amount0Min;
        uint256 amount1Min;
        uint256 deadline;
    }

    function decreaseLiquidity(
        DecreaseLiquidityParams calldata params
    ) external payable virtual returns (uint256 amount0, uint256 amount1);

    // set amount0Max and amount1Max to uint256.max to collect all fees
    struct CollectParams {
        uint256 tokenId;
        address recipient;
        uint128 amount0Max;
        uint128 amount1Max;
    }

    function collect(
        CollectParams calldata params
    ) external payable virtual returns (uint256 amount0, uint256 amount1);

    function burn(uint256 tokenId) external payable virtual;

    function positions(
        uint256 tokenId
    )
        external
        view
        virtual
        returns (
            uint96 nonce,
            address operator,
            address token0,
            address token1,
            uint24 fee,
            int24 tickLower,
            int24 tickUpper,
            uint128 liquidity,
            uint256 feeGrowthInside0LastX128,
            uint256 feeGrowthInside1LastX128,
            uint128 tokensOwed0,
            uint128 tokensOwed1
        );

    function balanceOf(
        address owner
    ) external view virtual returns (uint256 balance);
}
```

*ADAPTER CONTRACT*
```javascript
    // SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

import {IPancakeswapNonfungiblePositionManager} from "./Interfaces.sol";
import {RouterIntentEoaAdapterWithoutDataProvider, EoaExecutorWithoutDataProvider} from "@routerprotocol/intents-core/contracts/RouterIntentEoaAdapter.sol";
import {Errors} from "@routerprotocol/intents-core/contracts/utils/Errors.sol";
import {IERC20, SafeERC20} from "../../../utils/SafeERC20.sol";

/**
 * @title PancakeswapMint
 * @notice Minting a new position on Pancakeswap.
 */
contract PancakeswapMint is RouterIntentEoaAdapterWithoutDataProvider, PancakeswapHelpers {
    using SafeERC20 for IERC20;
    IPancakeswapNonfungiblePositionManager
        public immutable nonFungiblePositionManager;

    constructor(
        address __native,
        address __wnative,
        address __nonFungiblePositionManager
    )
        RouterIntentEoaAdapterWithoutDataProvider(__native, __wnative)
        PancakeswapHelpers(__nonFungiblePositionManager)
    // solhint-disable-next-line no-empty-blocks
    {
        nonFungiblePositionManager = IPancakeswapNonfungiblePositionManager(
            __nonFungiblePositionManager
        );
    }

    function name() public pure override returns (string memory) {
        return "PancakeswapMint";
    }

    /**
     * @inheritdoc EoaExecutorWithoutDataProvider
     */
    function execute(
        bytes calldata data
    ) external payable override returns (address[] memory tokens) {
        IPancakeswapNonfungiblePositionManager.MintParams
            memory mintParams = parseInputs(data);

        // If the adapter is called using `call` and not `delegatecall`
        if (address(this) == self()) {
            if (mintParams.token0 != native())
                IERC20(mintParams.token0).safeTransferFrom(
                    msg.sender,
                    self(),
                    mintParams.amount0Desired
                );
            else
                require(
                    msg.value == mintParams.amount0Desired,
                    Errors.INSUFFICIENT_NATIVE_FUNDS_PASSED
                );

            if (mintParams.token1 != native())
                IERC20(mintParams.token1).safeTransferFrom(
                    msg.sender,
                    self(),
                    mintParams.amount1Desired
                );
            else
                require(
                    msg.value == mintParams.amount1Desired,
                    Errors.INSUFFICIENT_NATIVE_FUNDS_PASSED
                );
        } else {
            if (mintParams.amount0Desired == type(uint256).max)
                mintParams.amount0Desired = getBalance(
                    mintParams.token0,
                    address(this)
                );

            if (mintParams.amount1Desired == type(uint256).max)
                mintParams.amount1Desired = getBalance(
                    mintParams.token1,
                    address(this)
                );
        }

        if (mintParams.token0 == native()) {
            convertNativeToWnative(mintParams.amount0Desired);
            mintParams.token0 = wnative();
        }

        if (mintParams.token1 == native()) {
            convertNativeToWnative(mintParams.amount1Desired);
            mintParams.token1 = wnative();
        }

        IERC20(mintParams.token0).safeIncreaseAllowance(
            address(nonFungiblePositionManager),
            mintParams.amount0Desired
        );

        IERC20(mintParams.token1).safeIncreaseAllowance(
            address(nonFungiblePositionManager),
            mintParams.amount1Desired
        );

        bytes memory logData;

        (tokens, logData) = _mint(mintParams);

        emit ExecutionEvent(name(), logData);
        return tokens;
    }

    //////////////////////////// ACTION LOGIC ////////////////////////////

    function _mint(
        IPancakeswapNonfungiblePositionManager.MintParams memory mintParams
    ) internal returns (address[] memory tokens, bytes memory logData) {
        (uint256 tokenId, , , ) = nonFungiblePositionManager.mint(mintParams);

        tokens = new address[](2);
        tokens[0] = mintParams.token0;
        tokens[1] = mintParams.token1;

        logData = abi.encode(mintParams, tokenId);
    }

    /**
     * @dev function to parse input data.
     * @param data input data.
     */
    function parseInputs(
        bytes memory data
    )
        public
        pure
        returns (IPancakeswapNonfungiblePositionManager.MintParams memory)
    {
        return
            abi.decode(
                data,
                (IPancakeswapNonfungiblePositionManager.MintParams)
            );
    }

    // solhint-disable-next-line no-empty-blocks
    receive() external payable {}
}
```

## Step 5: Compile the contract

```bash
npx hardhat compile
```

Ensure there are no compilation errors.

## Step 6: Deploy the Adapter

1. **Create a deployment script in scripts/deploy.js:**

```javascript
const hre = require("hardhat");

async function main() {
  const StaderStakeEth = await hre.ethers.getContractFactory("StaderStakeEth");
  const staderStakeEth = await StaderStakeEth.deploy(
    "0xNativeTokenAddress", // FIXED: Replace with actual native token address
    "0xWNativeTokenAddress", // FIXED: Replace with actual wrapped native token address
    "0xEthxTokenAddress", // PROTOCOL SPECIFIC NEED 1: Replace with the token address or pool address
    "0xStaderPoolAddress" // PROTOCOL SPECIFIC NEED 2: if any
  );

  await staderStakeEth.deployed();
  console.log("StaderStakeEth deployed to:", staderStakeEth.address);
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

2. **Deploy the contract:**

```bash
npx hardhat run scripts/deploy.js --network <your-network>
```

## Step 7: Interact with the Adapter

Once deployed, you can interact with the adapter using the following steps:

- Contact us for getting the adapter whitelisted on our Batch Transaction Contract in order to use the intents framework.
- The intent adapters can be interacted with the  Router's Batch Transaction Contracts deployed on every chain. Deployed addressed can be found [here](https://store.routerintents.com/adapters/batchHandler).

```javascript
/**
     * @dev function to execute batch calls on the same chain
     * @param appId Application Id
     * @param tokens Addresses of the tokens to fetch from the user
     * @param amounts amounts of the tokens to fetch from the user
     * @param feeData data for fee deduction
     * @param target Addresses of the contracts to call
     * @param value Amounts of native tokens to send along with the transactions
     * @param callType Type of call. 1: call, 2: delegatecall
     * @param data Data of the transactions
     */
    function executeBatchCallsSameChain(
        uint256 appId,
        address[] calldata tokens,
        uint256[] calldata amounts,
        bytes calldata feeData,
        address[] calldata target,
        uint256[] calldata value,
        uint256[] calldata callType,
        bytes[] calldata data
    ) external payable {}
```

The data can be created in the following way in order to call any adapter. Note that the protocol data is subject to change according to the needs of the action, here we are just giving an example for how to create data for Stader. The fee data contains following things : 

```javascript
    (
        uint256[] memory _appId,
        uint96[] memory _fee,
        address[] memory _tokens,
        uint256[] memory _amounts,
        bool _isActive
    )
```

1. `_appId`: Array of App ids for the specific protocols that you want to invoke as adapters in the transaction.
2. `_fee`: Array of the fees that has to be charged by the protocols in the same order.
3. `_tokens`: Array of the tokens needed for the protocols in that order.
4. `_amounts`: Array of amounts for the above tokens.
5. `_isActive`: If **true** , the adapters are subject to fees charged by Router.

```javascript 
    const amount = ethers.utils.parseEther("1");

    const staderData = defaultAbiCoder.encode(
      ["address", "uint256"],
      [deployer.address, amount]
    );

    const feedata = defaultAbiCoder.encode(
        ["uint256[]", "uint96[]", "address[]", "uint256[]", "bool"],
        [0], [0], [NATIVE_TOKEN], [amount], [true] 
    );

    const tokens = [NATIVE_TOKEN];
    const amounts = [amount];
    const targets = [staderStakeEthAdapter.address];
    const data = [staderData];
    const value = [0];
    const callType = [2];
    // const feeInfo = [{ fee: 0, recipient: zeroAddress() }];

    const balBefore = await ethers.provider.getBalance(deployer.address);
    const ethxBalBefore = await ethx.balanceOf(deployer.address);

    await batchTransaction.executeBatchCallsSameChain(
      0,
      tokens,
      amounts,
      feeData,
      targets,
      value,
      callType,
      data,
      { value: amount }
    );
```

## Conclusion

This guide demonstrates how to create a generic intent adapter using Router Protocol's Intents package. Adapt the contract logic to the protocol by modifying the constructor, external interfaces, and the core functionality (e.g., staking, swapping, adding liquidity, lending).
Happy building!

---

### Notes:

- You can modify the contract to suit different protocols by adjusting the external contract interface, parsing in `execute` function and logic inside the `internal` function.
- Make sure to update the deployment addresses (like `native`, `wnative`, and protocol-specific contract addresses) in the deployment script.
- Contact us for getting the adapter whitelisted on our Batch Transaction Contract in order to use the intents framework.
