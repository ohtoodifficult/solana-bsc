# Solana-BSC Genesis Contracts

This repository contains the genesis contracts that enable interoperability between **Solana** and **BNB Smart Chain (BSC)**. These contracts are crucial for cross-chain communication, asset transfers, and decentralized applications spanning both networks.

## 📌 Overview

This project provides:
- Smart contracts for **Solana** and **BSC** to enable seamless cross-chain operations.
- Scripts for generating **genesis configurations**.
- Automated tools for contract deployment and testing.

## 🛠️ Setup

### 1️⃣ Install Dependencies
```sh
npm install
```

### 2️⃣ Install Foundry (for BSC contracts)
```sh
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge install --no-git --no-commit foundry-rs/forge-std@v1.7.3
```

### 3️⃣ Install Anchor (for Solana contracts)
```sh
cargo install --git https://github.com/coral-xyz/anchor avm --locked
avm install latest
avm use latest
```

### 4️⃣ Install Poetry (for Python scripts)
```sh
curl -sSL https://install.python-poetry.org | python3 -
poetry install
```

### 5️⃣ Manage Node.js Versions (Optional)
```sh
# Install nvm and specific Node.js version
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.2/install.sh | bash
nvm install 12.18.3 && nvm use 12.18.3
```

## 🚀 Running Unit Tests

Before testing, set up your `.env` file:

```text
RPC_BSC=${archive_node}
RPC_SOLANA=${solana_rpc_node}
```

You can get free RPC endpoints from:
- **BSC:** [https://nodereal.io/](https://nodereal.io/)
- **Solana:** [https://www.quicknode.com/](https://www.quicknode.com/)

Run tests:
```sh
# Test BSC contracts
forge test

# Test Solana programs
anchor test
```

## 🏗️ Generating Genesis File

1. Modify `init_holders.js` to allocate initial BNB & SOL holders.
2. Modify `validators.js` for initial validator setup.
3. Adjust system contract settings as needed.
4. Run:
   ```sh
   node scripts/generate-genesis.js
   ```

## 🔄 Generating Genesis File for Different Networks

```sh
poetry run python -m scripts.generate ${network}
```

For details:
```sh
poetry run python -m scripts.generate --help
```

## 📄 Flattening System Contracts

```sh
bash scripts/flatten.sh
```

All system contracts will be flattened and output into `${workspace}/contracts/flattened/`.

## 🔄 Cross-Chain Asset Unlock (BEP-171)
```sh
npm install -g ts-node

cp .env.example .env
# Set UNLOCK_RECEIVER and OPERATOR_PRIVATE_KEY

ts-node scripts/bep171-unlock-bot.ts
```

## 🛠 Updating Contract Interfaces

### For BSC Contracts:
```sh
# Get metadata
forge build

# Generate interface
cast interface ${workspace}/out/{contract_name}.sol/${contract_name}.json -p ^0.8.0 -n ${contract_name} > ${workspace}/test/utils/interface/I${contract_name}.sol
```

### For Solana Programs:
```sh
anchor build
solana program dump -u ${network} ${program_id} > ${workspace}/idl/${program_name}.json
```

## 📜 License

The project is licensed under the [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0).
