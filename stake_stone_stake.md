# Stake ETH on StakeStone

The StakeStone Stake ETH adapter outlines a feature that enables users holding funds on any compatible chain to stake funds on StakeStone (on Ethereum) in one step. User can also choose the chain on which the funds(STONE OFT) should be received after liquid staking.

## Components of the StakeStone Stake ETH Adapter contract

**1. Constructor:** The constructor takes and sets the addresses of native token, wrapped native token, address of the stone vault along with the address of stone token for the respective chain.

```solidity
    constructor(
        address __native,
        address __wnative,
        address __stoneVault,
        address __stone
    )
```

**2. "execute" Function:** This function is present in every adapter contract and is expected to handle the data received from the multi caller(batch transaction contract).

- **Step 1:** Decodes the data received. Here the data includes address of the recipient, amount of ETH to be staked, network ID identifier (layer zero ID) of the final destination chain where the user wants to receive the funds(STONE) and crosschain data for that respective chain. The crosschain data consists of ABI-encoded native fee and refund address.

```javascript
    (         
            address _recipient, 
            uint256 _amount, 
            uint16 dstEid, 
            bytes memory crossChainData
    )
```

- **Step 2:** Receives the respective funds.

- **Step 3:** Calls the *_stake* function.

**3. "_stake" Function:** This internal function is responsible for calling the StakeStone Vault, staking the funds on StakeStone and transferring the StakeStone asset to the user/recipient on the chain specified by them in the execution data.
