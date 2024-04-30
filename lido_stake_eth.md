# Stake ETH on Lido

The Lido Stake ETH adapter outlines a feature that enables users holding funds on any compatible chain to stake funds on Lido (on Ethereum) in one step. User can also choose the chain on which the funds should be received after liquid staking from among the supported chains. 

## Components of the Lido Stake ETH Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of required tokens and gateways in the following manner: 

```solidity
    constructor(
        address __native,
        address __wnative,
        address __lidoStETH,
        address __lidoWstETH,
        address __referralId,
        address __arbitrumGateway,
        address __baseGateway,
        address __lineaGateway,
        address __mantleGateway,
        address __optimismGateway,
        address __zksyncGateway,
        address __lidoWstEthOptimism,
        address __lidoWstEthBase,
        address __lidoWstEthMantle
    ) {}
```

The deployments addresses for various gateways supported by Lido can be found [here](https://docs.lido.fi/deployed-contracts/).

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data includes address of the recipient, amount of ETH to be staked, network ID of the final destination chain where the user wants to receive the funds(wstETH) and data for the respective bridge ID.

```javascript
    (         
            address _recipient,
            uint256 _amount,
            uint256 _bridgeChainId,
            bytes memory _bridgeData
    )
```

- **Step 2:** Receives the respective funds.

- **Step 3:** Calls the *_stake* function.

**3. "_stake" Function:** This internal function is responsible for calling the Lido Vault, staking the funds on Lido and transferring the lido asset to the user/recipient on the chain specified by them in the execution data.
