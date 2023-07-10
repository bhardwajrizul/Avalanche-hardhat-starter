# Connecting to Avalanche with Hardhat

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

🌟 A comprehensive guide on connecting to Avalanche using Hardhat for seamless development and testing. 🚀

## Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Why Avalanche and Hardhat?](#why-avalanche-and-hardhat)
- [Different Chains on Avalanche](#different-chains-on-avalanche)
- [Folder Structure](#folder-structure)
- [Setup and Configuration](#setup-and-configuration)
- [Deploying Smart Contracts](#deploying-smart-contracts)
- [Verifying Smart Contracts](#verifying-smart-contracts)
- [Conclusion](#conclusion)
- [License](#license)

## Introduction

Welcome to the guide on connecting to Avalanche, a high-performance blockchain platform, using Hardhat, a popular development environment for Ethereum smart contracts. This guide will walk you through the necessary steps to get started, deploy smart contracts, and interact with the Avalanche ecosystem.

## Prerequisites

Before you begin, ensure you have the following prerequisites:

1. Node.js installed (version X.X.X or higher)
2. Basic knowledge of Ethereum and smart contracts
3. Familiarity with the command line interface (CLI)

## Why Avalanche and Hardhat?

**Avalanche** is a next-generation blockchain platform that provides high scalability, low fees, and fast transaction finality. It offers a rich ecosystem for building decentralized applications, DeFi protocols, and various other use cases. Avalanche's flexibility, interoperability, and robustness make it an attractive choice for developers.

**Hardhat** is a powerful development environment for Ethereum smart contracts. While it is primarily designed for Ethereum, it can be configured to work seamlessly with Avalanche. Hardhat offers advanced features like testing, debugging, and deployment automation, making it an ideal choice for Avalanche development.


## Different Chains on Avalanche

Avalanche supports multiple chains, each serving different purposes within its ecosystem. Here are some of the prominent chains on Avalanche:



- **Exchange** Chain, or the **X-Chain**, is where all the action happens with digital smart assets known as Avalanche Native Tokens.

- **Contract** Chain, or the **C-Chain**, is an implementation of the Ethereum Virtual Machine (EVM) where you can flex your Solidity skills and create and execute smart contracts.

- **Platform** Chain, or the **P-Chain**, is where the magic happens 🧙‍♂️, supporting the creation of new blockchains and subnets, staking operations, and more.


-  **Fuji Chain**: The Fuji Chain is Avalanche's  testnet, allowing developers to test their applications and smart contracts in a safe and controlled environment. It replicates the functionality of the mainnet but operates on test tokens to prevent real asset loss.

### 🏔️ Fuji Chain

Fuji Chain is Avalanche's dedicated testnet environment, designed specifically for developers and testers. It provides a sandboxed network where you can experiment, deploy contracts, and test applications without incurring real financial risks. The Fuji Chain closely mirrors the mainnet's architecture and functionality, allowing developers to ensure the reliability and correctness of their code before deploying it to the live network.

### 💦 Testnet Faucet

A testnet faucet is a tool or service that provides developers with free test tokens or cryptocurrencies for use on testnets like the Avalanche Fuji Chain. These test tokens have no real value and are solely meant for testing and development purposes. Testnet faucets help developers acquire the necessary tokens to deploy, interact with smart contracts, and simulate real-world scenarios without spending actual funds.
You can get testnet funds here :https://core.app/tools/testnet-faucet/?subnet=c&token=c

## Folder Structure

Before we dive into the setup process, let's take a look at the recommended folder structure for your Avalanche project:

```
├── contracts/            # Contains your smart contracts
├── scripts/              # Deployment and interaction scripts
├── test/                 # Test cases for your smart contracts
├── hardhat.config.js     # Hardhat configuration file
├── README.md             # Project documentation
└── ...
```

This structure helps organize your project and separates the different components of your Avalanche development workflow.

## Setup and Configuration

### Step 1: Project Setup

1. Create a new folder for your project and navigate into it:

```shell
$ mkdir my-avalanche-project
$ cd my-avalanche-project
```

2. Initialize the project and install Hardhat as a development dependency:

```shell
$ npm init -y
$ npm install --save-dev hardhat
```

3. Initialize your Hardhat project:

```shell
$ npx hardhat
```

- Select the option  
`❯ Create a JavaScript project.`
- This will generate a Hardhat config file (e.g. `hardhat.config.js`).

4. Install the Hardhat Toolbox plugin:

```shell
$ npm i --save-dev @nomicfoundation/hardhat-toolbox
```

The Hardhat Toolbox plugin (@nomicfoundation/hardhat-toolbox) is a useful addition to the Hardhat development environment. 
It offers various utility tasks and helpers that enhance smart contract development on a network. It provides features such as contract interaction and testing utilities, deployment automation, network management, task automation, and an improved development experience. 
By using the Hardhat Toolbox plugin, you can streamline your development workflow, automate common tasks, and improve efficiency when working with smart contracts on Avalanche.

### Step 2: Project Configuration

Modify your `hardhat.config.js` file to include Avalanche configurations:

> When using the hardhat network, you may choose to fork Fuji or Avalanche Mainnet. This will allow you to debug contracts using the hardhat network while keeping the current network state. To enable forking, turn one of these booleans on, and then run your tasks/scripts using --network hardhat

```javascript
require("@nomicfoundation/hardhat-toolbox");

const FORK_FUJI = false;
const FORK_MAINNET = false;
let forkingData = undefined;

if (FORK_MAINNET) {
  forkingData = {
    url: "https://api.avax.network/ext/bc/C/rpcc",
  };
}
if (FORK_FUJI) {
  forkingData = {
    url: "https://api.avax-test.network/ext/bc/C/rpc",
  };
}

module.exports = {
  solidity: "0.8.18",
  networks: {
    hardhat: {
      gasPrice: 225000000000,
      chainId: !forkingData ? 43112 : undefined,
      forking: forkingData
    },
    fuji: {
      url: 'https://api.avax-test.network/ext/bc/C/rpc',
      gasPrice: 225000000000,
      chainId: 43113,
      accounts: [
        // YOUR PRIVATE KEY HERE
      ]
    },
    mainnet: {
      url: 'https://api.avax.network/ext/bc/C/rpc',
      gasPrice: 225000000000,
      chainId: 43114,
      accounts: [
        // YOUR PRIVATE KEY HERE
      ]
    }
  }
}
```
- url: The URL specifies the endpoint to connect to the Fuji testnet. It is set to 'https://api.avax-test.network/ext/bc/C/rpc'.

- gasPrice: The gas price determines the cost of executing transactions on the network. For Fuji, it is set to 225000000000.

- chainId: The chain ID uniquely identifies the Fuji testnet on Avalanche. It is set to 43113.

- accounts: This is where you need to provide your private key. Add your private key as a string within the array. Private keys are used to sign transactions and prove ownership of accounts.


**Note**: Replace `YOUR PRIVATE KEY HERE` with your actual private key for the desired network.

## Deploying Smart Contracts



1. In the `scripts/` directory, create a deployment script, e.g., `deploy.js`. Use the following template:

```javascript
const hre = require("hardhat");

async function main() {
	
  const contractName == "YOUR CONTRACT NAME" 
  const contract = await hre.ethers.deployContract(contractName);

  await contract.waitForDeployment();

  console.log(`Contract Deployed to address ${await contract.getAddress()}`);
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

> To deploy our smart contract, we need to get some Testnet AVAX from the Faucet. You can get that here: https://faucet.avax.network/.

2. Write the deployment logic within the `main()` function, using Hardhat's deployment APIs.

3. Run the deployment script on the desired network


```shell
$ npx hardhat run scripts/deploy.js --network fuji
```

**Note**: Replace `fuji` with the desired network name. 
> You need to have some AVAX tokes in your account before deploying the contract on Avalanche

## Verifying Smart Contracts

1. To verify your deployed smart contracts using hardhat  on Avalanche, you'll need an API key from [Snowtrace](https://snowtrace.io/). Sign up on their website and obtain your API key.

2. Add this to your `hardhat.config.js` after generating your API key and modify the `etherscan` section:

```javascript

module.exports = {
  // ...rest of the config...
  etherscan: {
    apiKey: "YOUR_API_KEY",
  },
};
```

3. Once configured, you can use the following command to verify your smart contracts:

```shell
$ npx hardhat verify <contract_address> --network fuji
```

> Generally you can run this command to verify your contract `npx hardhat verify <contract address> <arguments> --network <network>`


**Note**: Replace `<contract_address>` with the actual address of your deployed smart contract.

## Conclusion

Congratulations! You have successfully connected to Avalanche using Hardhat. Now you can start developing, deploying, and interacting with smart contracts on the Avalanche blockchain. Feel free to explore the rich ecosystem of Avalanche and leverage the power of Hardhat for seamless development and testing.

For detailed usage instructions and additional functionalities, refer to the Hardhat and Avalanche documentation.

## License

This project is licensed under the [MIT License](LICENSE). Feel free to use and modify the code according to your needs.

---