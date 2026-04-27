# Merkle Airdrop Extravaganza

This repository is part of the **Cyfrin Updraft Advanced Foundry Course**.  
It demonstrates how to implement and interact with **Merkle trees, signatures, and airdrop distribution mechanisms** using Foundry and zkSync.

---

## What You’ll Learn

- How Merkle trees work for scalable airdrops
- How to generate and verify Merkle proofs
- Signature-based claim authorization
- Deploying smart contracts on:
  - Local Anvil chain
  - zkSync local node
  - zkSync Sepolia testnet
- Writing and testing secure Solidity contracts using Foundry

---

## Getting Started

### Requirements

Make sure you have the following installed:
```javascript
  - **git**
    ```bash
    git --version
```

Foundry
```javascript
    forge --version
```    

If not installed, follow: https://book.getfoundry.sh/

## Quickstart
```javascript
    git clone https://github.com/Cyfrin/foundry-merkle-airdrop-cu
    cd foundry-merkle-airdrop-cu
    make
    # or
    forge install && forge build
```

## Usage Overview
### Pre-deploy: Generate Merkle Proofs

If you want to use custom recipients:

Update whitelist in:
```javascript
    script/GenerateInput.s.sol
```

Generate Merkle root + proofs:
```javascript
    make merkle
```

OR:
```javascript
    forge script script/GenerateInput.s.sol:GenerateInput
    forge script script/MakeMerkle.s.sol:MakeMerkle
```

Retrieve the root from:
```javascript
    script/target/output.json
```

Update:
`Makefile` → `ROOT` (for zkSync)
`DeployMerkleAirdrop.s.sol` → `s_merkleRoot` (for Anvil/Ethereum)

## Deployment
### Deploy to Anvil (Local EVM)
```javascript
    foundryup
    make anvil
    make deploy
```    
## Deploy to zkSync Local Node
### Requirements:
foundry-zksync
npm & npx
docker

Setup zkSync node:
```javascript
    npx zksync-cli dev config
    npx zksync-cli dev start
```    

Or simply:
```javascript
    make zk-anvil
```

Deploy:
```javascript
    foundryup-zksync
    make deploy-zk
```    
## Deploy to zkSync Sepolia

Ensure:

`.env` contains `ZKSYNC_SEPOLIA_RPC_URL`
You have a configured `default` account in cast
```javascript
    foundryup-zksync
    make deploy-zk-sepolia
```

## Interacting with the System
 ### zkSync Local Interaction

Run full flow (deploy + claim):
```javascript
    chmod +x interactZk.sh
    ./interactZk.sh
```    
This script:

Deploys contracts
Signs claim
Executes airdrop claim

## Anvil Interaction (Local EVM)
1. Start chain + deploy
```javascript
    foundryup
    make anvil
    make deploy
```    

Copy:

Airdrop contract address
Token address

Update them in `Makefile`.

1. Sign claim
```javascript
    make sign
```

Copy signature into:
```javascript
    Interact.s.sol
```    

(Remove 0x prefix if needed)

1. Claim tokens
```javascript
    make claim
```    
1. Check balance
```javascript 
    make balance
```    
## Testing
Run tests
```javascript
    foundryup
    forge test
```

zkSync tests
```javascript
    make zktest
```

Test Coverage
```javascript
    forge coverage
```    
Gas Snapshot
```javascript
    forge snapshot
```

Output:
```javascript
    .gas-snapshot
```
## Formatting
```javascript
    forge fmt
```    
## Thank You

Built as part of the Cyfrin Advanced Foundry Course.
Focus: mastering real-world smart contract security patterns like:
    1. Merkle distributions
    2. Signature verification
    3. Cross-chain deployment workflows
    4. Secure token airdrops

## Key Concept

Instead of storing thousands of addresses on-chain, we store a single Merkle root and verify claims using Merkle proofs, making airdrops scalable and gas-efficient.