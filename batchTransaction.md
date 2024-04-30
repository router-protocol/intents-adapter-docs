
# BATCH-TRANSACTION (MULTI-CALLER) CONTRACT

The batch transaction contract acts as a versatile multi-caller contract, enabling the simultaneous execution of multiple transactions. It seamlessly integrates with Router's Cross-Chain Intents Framework (CCIF) Adapters, facilitating efficient cross-chain communication and interoperability. This capability enhances the contract's flexibility and utility, empowering users to engage in complex transactions across different chains with ease and efficiency.

## Key functions

**1. executeBatchCallsSameChain Function:** This function is used to execute batch calls on same chain. It is responsible for calling *execute* function on every adapter for triggering respective tasks such as staking, liquidity addition, lending, borrowing and so on. 

```javascript
    function executeBatchCallsSameChain(
        address[] calldata tokens,
        uint256[] calldata amounts,
        address[] calldata target,
        uint256[] calldata value,
        uint256[] calldata callType,
        bytes[] calldata data
    ) external payable {}
```
The parameters for this function are explained below:

- **a) tokens:** Array of addresses of the tokens to fetch from the user.
- **b) amounts:** Array of amounts of the tokens to fetch from the user.
- **c) target:** Array of addresses of the contracts/adapters to call.
- **d) value:** Array of amounts of native tokens to send along with the transactions.
- **e) callType:** Type of call. 1: call, 2: delegateCall.
- **f) data:** Array of ABI-encoded data for the respective transactions.

**2. handleMessage Function:** This function is used to handle cross-chain requests received from Router Nitro. It is also responsible for calling *execute* function on every adapter for triggering respective tasks such as staking, liquidity addition, lending, borrowing and so on. Note that only Router Nitro can call this function on destination chain.

```javascript
    function handleMessage(
        address tokenSent,
        uint256 amount,
        bytes memory instruction
    ) external override onlyNitro nonReentrant {}
```

The parameters for this function are explained below:

- **a) tokenSent:** Address of token received.
- **b) amount:** Amount of tokens received.
- **c) instruction:** Instruction passed from source chain to be executed on destination chain.

#### Example 1 : Add liquidity on Uniswap

![EXAMPLE-1](./assets/EXAMPLE_1.png)

- **Subcase 1 :** If the user doesn't have desired tokens:
    1. Swap to tokens A and B using the Dexspan adapter.
    2. Add liquidity on Uniswap using the Uniswap adapter. 

    This set of transactions can be executed through Batch transaction contract's *executeBatchCallsSameChain* function. 

    In the above example: the array of targets would be passed as *[address of dexspan adapter, address of uniswap adapter]*, and the *data* would be an array of respective inputs.


- **Subcase 2 :** If the user has desired tokens, they can add   liquidity on Uniswap using the Uniswap adapter directly. 

    In this case, the array of targets would be passed as *[address of uniswap adapter]* in the *executeBatchCallsSameChain* function along with other parameters accordingly.

#### Example 2 : Crosschain liquid stake ETH

![EXAMPLE-2A](./assets/EXAMPLE_2a.png)
![EXAMPLE-2B](./assets/EXAMPLE_2b.png) 

- If the user wants to stake Ethereum on a liquid staking protocol but has funds on some other chain, say Polygon:
    1. The user can bridge funds from Polygon to Ethereum using Nitro Adapter on Polygon.
    2. After the request is received on Ethereum , the funds automatically get staked on the desired Liquid Staking Protocol.

This set of transactions can be executed through Batch transaction contract's *executeBatchCallsSameChain* function on the source chain.    

In this case, the array of targets would be passed here as *[address of nitro adapter]* . The *data* parameter will contain inputs to the Nitro adapter. Among the inputs, there is a *message* parameter in which the user has to encode the data pertaining to liquid staking on ethereum. The message to be sent would consist the refund address, destination targets (LST Adapter address) and the data for that adapter/target.

    