# How to Build NFTs Using Hardhat on the Celo Blockchain
    

## Table of Contents
- [How to Build NFTs Using Hardhat on the Celo Blockchain](#how-to-build-nfts-using-hardhat-on-the-celo-blockchain)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Prerequisites](#prerequisites)
  - [Tech Stack](#tech-stack)
  - [Project Setup](#project-setup)
    - [Configuring Hardhat for the Celo Alfajores Testnet](#configuring-hardhat-for-the-celo-alfajores-testnet)
    - [Create .env File](#create-env-file)
  - [Write and Compile the Smart Contract](#write-and-compile-the-smart-contract)
    - [Write Smart Contract](#write-smart-contract)
    - [Configure Metadata IPFS](#configure-metadata-ipfs)
      - [Create Image folder](#create-image-folder)
      - [Create JSON File](#create-json-file)
    - [Compile NFT Smart Contract](#compile-nft-smart-contract)
  - [Test NFT Smart Contract](#test-nft-smart-contract)
  - [Deploy NFT Smart Contract](#deploy-nft-smart-contract)
  - [Minting NFT](#minting-nft)
  - [Import Smart Contract.](#import-smart-contract)
  - [Conclusion](#conclusion)


## Introduction

<img width="1200" alt="remix-capture" src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/celo-banner.png">


Non-fungible assets are non-interchangeable assets. Non-fungible assets are also unique and different. They can not be replaced by another asset - even an asset of the same value. Examples of non-fungible assets include digital collectibles, arts, houses, domain names, etc.

We can represent non-fungible assets on the blockchain as non-fungible tokens (NFTs). The authenticity and ownership of NFTs are easily verified on the blockchain.

In this tutorial, you will learn how to build NFTs using Hardhat and the [ERC721](https://eips.ethereum.org/EIPS/eip-721) token standard on the Celo blockchain. This tutorial will enable us to:

* Build NFTs projects in our local development environment without the [Remix IDE](https://remix.ethereum.org/) using Hardhat.
    
* We will also deploy it on the Celo Alfajores testnet using Hardhat.

>**_Note_**: Since Celo is an EVM-compatible blockchain, it can use the Ethereum token standards and its tools to build NFTs smart contracts. Learn more about these [token standards](https://ethereum.org/en/developers/docs/standards/tokens/#:~:text=Here%20are%20some%20of%20the,for%20artwork%20or%20a%20song.).


## Prerequisites

For a proper understanding of this tutorial, you need a basic understanding of the following:

* Blockchain technology.
    
* Basic understanding of blockchain technologies.
    
* Node.js. This tutorial requires version 12 or higher.
    
* An IDE. We will use [VS Code](https://code.visualstudio.com/download).
    
* [NFT contracts](https://hackmd.io/1_56InhcSO-oqrF8WXfhaQ).

* Familiarity with managing different node versions using a package. In this tutorial, we will use the [n](https://www.npmjs.com/package/n) package.
    

## Tech Stack

In this tutorial, we will use the following tools and resources:

* [Node JS](https://nodejs.org/en/) - A JavaScript runtime environment.
    
* [Hardhat](https://hardhat.org/) - A Ethereum development environment.
    
* [OpenZeppelin](https://www.openzeppelin.com/) Contracts - A library of secured smart contracts.
    


## Project Setup

To check your node version, open your terminal, and run the command below:

```bash
node -v
```

If your node version is less than version 12, upgrade using the command below. This command assumes you have the `n` package installed:

```bash
n latest
```

You can now verify that your node version upgrade is complete by running the command below.

```bash
node -v
```

With your terminal still opened, create a new folder called `NFTs-hardhat` by running the command below.

```bash
mkdir NFTs-hardhat
```

Navigate to **NFTs-hardhat** folder with the command below

```bash
cd  NFTs-hardhat
```

Initialize an NPM package by running the command below.

```bash
npm init
```

The command above will generate a `package.json` file. A `package.json` file describes the dependencies in our project. Each time you add a new dependency, the **package.json** file is updated to include the new dependency.

With your terminal still in the **NFTs-hardhat** directory, run the command below to open the **NFTs-hardhat** directory in VS Code:

```bash
code .
```

The NFTs-hardhat directory will have the package.json file we initialized earlier. Your VS Code interface will look like the image below.

<img src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/package-json-view.png?raw=true">

Next, we need to install Hardhat locally. Hardhat enables us to run, compile, debug, edit, test, and deploy our smart contract locally. Use the command below to install Hardhat on your machine.

```bash
npm install --save-dev hardhat
```

After the complete installation of Hardhat, your VS Code interface will now have a folder structure similar to the image below.

<img src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/hardhat.png?raw=true">

To use our local installation of Hardhat, run the command below. The command will initialize a sample project. We will build upon the sample project in this tutorial. The sample project will give us a good understanding of what is available in Hardhat.

```bash
npx hardhat
```

The command will prompt some questions on how to set up your project. Press the **enter** key for each prompt.

After completing the initialization of the sample project, You will see an instruction in your terminal that requires that you run the command below.

```bash
 npm install --save-dev "hardhat@^2.12.7" "@nomicfoundation/hardhat-toolbox@^2.0.0"
```

The command above installs the `@nomicfoundation/hardhat-toolbox` plugin that enables us to perform some actions such as:

* Deploy your smart contract.
    
* Interact with your smart contract using the **ether.js** library.
    
* Test your smart contract.
    
* Interact with the Hardhat network.
    

To see a list of available tasks, run the command below

```bash
npx hardhat
```

### Configuring Hardhat for the Celo Alfajores Testnet

We need to configure Hardhat to enable it to deploy on the Celo Alfajores testnet. To do that open the `hardhat.config.js` file, located in the root folder. Afterward, replace the content of the `hardhat.config.js` file with the code below.

```js
require("@nomicfoundation/hardhat-toolbox");
require("dotenv").config({ path: ".env" });

/** @type import('hardhat/config').HardhatUserConfig */
module.exports = {
  solidity: "0.8.9",
  networks: {
    alfajores: {
      url: "https://alfajores-forno.celo-testnet.org",
      accounts: {
        mnemonic: process.env.MNEMONIC, // line 25
        path: "m/44'/60'/0'/0", // line 26
      },
      chainId: 44787,
    },
  },
};
```

In the code above, we have a [module.exports](https://www.sitepoint.com/understanding-module-exports-exports-node-js/) which is essentially exporting an object that contains the following properties:

* `solidity`: This accepts the value of our smart contract Solidity version.
    
* `networks`: This contains the network we are deploying to, in this case, it is the Alfajores testnet.
    
* `alfajores`: This accepts the Alfajores testnet URL and accounts.
    
* `accounts`: This accepts the details of the Celo Alfajores testnet account that we need to deploy our smart contract. The account property accepts an object with two properties, namely:
    
    * `mnemonic`: This accepts a mnemonic variable from a .`env` file, which is required at the top of our code. This **mnemonic** is our wallet recovery secret phrase which we can use to grant Hardhat access to our account. This enables Hardhat to interact with the Alfajores testnet. We will create a `.env` file shortly.
        
    * `path`: This is also called a [derivation path](https://medium.com/mycrypto/wtf-is-a-derivation-path-c3493ca2eb52). The path provided above is a Metamask path. If you are using the **Celo Extension Wallet**, change the path to this `m/44'/52752'/0'/0`.
        
* `chainId`: Also called a chain identifier. Accepts the Celo chain ID.
    

### Create .env File

Seed phrases are meant to be private. We will store our seed phrase in a `.env` file. The `.env` file prevents the upload of our seed phrase.

Run the command below to install the `dotenv` dependency.

```bash
npm install dotenv
```

After the complete installation of the `dotenv` package, navigate to the root directory, and create a `.env` file. Open your Metamask wallet, and click on the identicon, located at the top right corner. Select **settings**, then select **security and privacy**. Click on ***Reveal Secret Recovery Phrase***. Copy the phrase, and paste it into the newly created `.env` file. Your env file should look like the image below

```bash
MNEMONIC="YOUR_SECRET_RECOVERY_PHRASE"
```

Replace the value of the mnemonic variable with the phrase you copied from your Metamask wallet.

Now, test if Hardhat is working as intended by running the command below.

```bash
npx hardhat run scripts/deploy.js --network alfajores
```

The command above deploys the **Lock.sol** smart contract located inside the `contracts` folder which was automatically added by Hardhat during the initialization of our project. You will receive a response similar to the below image in your terminal.

<img src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/deploy.png?raw=true">



## Write and Compile the Smart Contract

Now that we are done with our project setup. We will now write and compile our smart contract.

### Write Smart Contract

In this section, we will write our smart contract using the Solidity programming language. Follow the steps below to create a file for our smart contract.

1. Open the `contracts` folder at the root of your directory
    
2. Delete the `Lock.sol` file.
    
3. Create a new file under the contracts folder and name the file `MyNFT.sol`
    
4. Copy the code below into the file.
    

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Enumerable.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Burnable.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/Counters.sol";

contract MyNFT is ERC721, ERC721Enumerable, ERC721URIStorage, ERC721Burnable, Ownable {
    using Counters for Counters.Counter;

    Counters.Counter private _tokenIdCounter;

    constructor() ERC721("AfricaPrintNFT", "APNFT") {}

    function _baseURI() internal pure override returns (string memory) {
        return "https://ipfs.io/ipfs/QmXsvFkvY9FL3bNwx8P1BqpdzHQxUgLByo8FP5YqQ39cKN/";
    }

    function safeMint(address to, string memory uri) public onlyOwner {
        uint256 tokenId = _tokenIdCounter.current();
        _tokenIdCounter.increment();
        _safeMint(to, tokenId);
        _setTokenURI(tokenId, uri);
    }

    // The following functions are overrides required by Solidity.

    function _beforeTokenTransfer(address from, address to, uint256 tokenId, uint256 batchSize)
        internal
        override(ERC721, ERC721Enumerable)
    {
        super._beforeTokenTransfer(from, to, tokenId, batchSize);
    }

    function _burn(uint256 tokenId) internal override(ERC721, ERC721URIStorage) {
        super._burn(tokenId);
    }

    function tokenURI(uint256 tokenId)
        public
        view
        override(ERC721, ERC721URIStorage)
        returns (string memory)
    {
        return super.tokenURI(tokenId);
    }

    function supportsInterface(bytes4 interfaceId)
        public
        view
        override(ERC721, ERC721Enumerable)
        returns (bool)
    {
        return super.supportsInterface(interfaceId);
    }
}
```

You can generate the above code using the [OpenZeppelin](https://docs.openzeppelin.com/contracts/4.x/erc721) open-source library. Let us have a breakdown of the above code by explaining each piece of code.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Enumerable.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Burnable.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/Counters.sol";
```

On the first line is a comment indicating the [license](https://spdx.dev/) of our source file. Below the license is a `pragma` keyword that is required to determine the solidity version the compiler should use during compilation. From the above, we can see that our source file requires a compiler with a version greater than or equal to **0.8.9** but less than version **0.9.0**.

```solidity
contract MyNFT is ERC721, ERC721Enumerable, ERC721URIStorage, ERC721Burnable, Ownable {}
```

We created the `MyNFT` contract, which inherits the libraries imported from OpenZeppelin. These libraries contain functionalities or methods governed by the [ERC721](https://eips.ethereum.org/EIPS/eip-721) standard.

The `Ownable` keyword sets access control on our smart contract, this allows only the owner to mint NFTs.

```solidity
using Counters for Counters.Counter;
Counters.Counter private _tokenIdCounter;
```

In the code above, we attach every function in Counters to `Counters.Counter`. It has an `increment()` function that allows us to track the number of NFTs minted.

```solidity
constructor() ERC721("AfricaPrintNFT", "APNFT") {}
```

The constructor ERC721 has two `string` passed into it. The constructor sets the two `string` on deployment. It sets:

* **AfricaPrintNFT** as the name of our NFT.
    
* **APNFT** as the symbol of our NFT.
    
We will now define the `_baseURI()` function:

```solidity
function _baseURI() internal pure override returns (string memory) {
        return "https://ipfs.io/ipfs/QmXsvFkvY9FL3bNwx8P1BqpdzHQxUgLByo8FP5YqQ39cKN/";
    }
```

The `_baseURI()` function returns a link that points to a JSON file that contains the metadata of our tokens. This metadata includes the description, image link, and names of our tokens. If we are to mint a single token, then a token URI is enough, more on how to set your `baseURI` later.

>**_Note_**: The `_setTokenURi()` function we will use in the `safeMint()` function will concatenate the **Base URI** with the argument we use for the `uri` parameter.

Next, we will define the `safeMint()` function:

```solidity
function safeMint(address to, string memory uri) public onlyOwner {
        uint256 tokenId = _tokenIdCounter.current();
        _tokenIdCounter.increment();
        _safeMint(to, tokenId);
        _setTokenURI(tokenId, uri);
    }
```

The `safeMint()` function allows the minting of tokens to the address passed through the `to` parameter and uses the `uri` parameter in the `_setTokenURI()` method to set the `uri` of a token. If you want to learn more about the `_safeMint()` and `_setTokenURI()` functions, the [OpenZeppelin docs](https://docs.openzeppelin.com/contracts/2.x/api/token/erc721) is a great place to start.

Finally, we will add all the **overrides** required by Solidity:

```solidity
function _beforeTokenTransfer(address from, address to, uint256 tokenId, uint256 batchSize)
        internal
        override(ERC721, ERC721Enumerable)
    {
        super._beforeTokenTransfer(from, to, tokenId, batchSize);
    }

    function _burn(uint256 tokenId) internal override(ERC721, ERC721URIStorage) {
        super._burn(tokenId);
    }

    function tokenURI(uint256 tokenId)
        public
        view
        override(ERC721, ERC721URIStorage)
        returns (string memory)
    {
        return super.tokenURI(tokenId);
    }

    function supportsInterface(bytes4 interfaceId)
        public
        view
        override(ERC721, ERC721Enumerable)
        returns (bool)
    {
        return super.supportsInterface(interfaceId);
    }
```

The above functions are overrides required by Solidity. The functions are inherited from the ERC721 OpenZeppelin library. Learn more about these functions [here](https://docs.openzeppelin.com/contracts/4.x/api/token/erc721).

### Configure Metadata IPFS

In this section, we will look at how you can configure your metadata. The metadata is a JSON file that contains information such as the description, image link, and name of our tokens.

We will store the metadata and images for our token in a decentralized way using the [InterPlanetary File System(IPFS)](https://docs.ipfs.tech/concepts/what-is-ipfs/) and [Pinata](https://www.pinata.cloud/).

#### Create Image folder

Create a folder named **img** with 3 images in the format below:

```markdown
img
├── nft_1.png
├── nft_2.png
├── nft_3.png
```

We need to upload the images above in a decentralized way using Pinata to host them on IPFS. To pin your images, follow the steps below:

1. Open [Pinata](https://app.pinata.cloud/) in a new tab.
    
2. Sign up. After signing up, Pinata will redirect us to the Pinata pinmanger
    
3. Click on the plus icon with add file text. Click on **upload** folder, then upload the **img** folder.
    

After a successful upload, you will see your images with their **content identifier(CID)**. You can access these images by appending the CID of respective images to this URL- `https://ipfs.io/ipfs/${CID}`. Replace `${CID}` with the CID of any of the images you wish to access.

#### Create JSON File

The JSON file contains metadata for each image.

The **metadata** folder will contain three JSON file that represents the metadata for each image.

Create a folder, and name it metadata in the format below:

```markdown
metadata
├── 0
├── 1
├── 2
```

The content of each JSON should look like the format below. You can modify it as you deem fit.

```json
{
    "name": "nft0",
    "description": "NFT collection for educational purposes.",
    "image": "https://ipfs.io/ipfs/QmSaMQS7o1Qj68si6vUz4nHyQu8PdPwtcwNgASY79kqXxb/comrade1.jpg/geo_1.png",
    "attributes": [
        { "trait_type": "background", "value": "cyan" },
        { "trait_type": "color", "value": "red" },
        { "trait_type": "shape", "value": "circle" }
    ]
}
```

Create metadata similar to the one above for file 0, file 1, and file 2.

Upload the metadata folder on Pinata. the CID of the metadata folder on Pinata will form the `baseURI` of our NFTs.

Replace the base URI in `MyNFT.sol` with your base URI.

### Compile NFT Smart Contract

One of the functionalities of Hardhat is that we can compile our code right in our development environment. Follow the instruction below to compile your code:

* Open your terminal
    
* Navigate to the root directory of our project.
    
* Run `npm install @openzeppelin/contracts`. This will install dependencies necessary for the OpenZeppelin contract we imported to run.
    
* After the complete installation of @openzeppelin, run `npx hardhat compile`. This will compile our smart contract.
    
* You will see a response similar to the image below.
    

<img src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/compile.png?raw=true">

## Test NFT Smart Contract

Let's write a test to confirm that our smart contract is working as expected. Follow the instruction below to create a file for our test:

* Navigate to the test folder in the root directory.
    
* Delete the `test.js` file.
    
* Create a new file under the test folder. Name the file `nft-test.js`.
    
* Copy and paste the code below into the `nft-test.js` file.
    

```javascript
const { expect } = require("chai");
const { ethers } = require("hardhat");

describe("MyNFT", function () {
  this.timeout(50000);

  let myNFT;
  let owner;
  let acc1;
  let acc2;

  this.beforeEach(async function() {
      // This is executed before each test
      // Deploying the smart contract
      const MyNFT = await ethers.getContractFactory("MyNFT");
      [owner, acc1, acc2] = await ethers.getSigners();

      myNFT = await MyNFT.deploy();
  })

  it("Should set the right owner", async function () {
      expect(await myNFT.owner()).to.equal(owner.address);
  });

  it("Should mint one NFT", async function() {
      expect(await myNFT.balanceOf(acc1.address)).to.equal(0);
      
      const tokenURI = "https://example.com/1"
      const tx = await myNFT.connect(owner).safeMint(acc1.address, tokenURI);
      await tx.wait();

      expect(await myNFT.balanceOf(acc1.address)).to.equal(1);
  })

  
});
```

Let's break down the above code into small different sections.

```javascript
const { expect } = require("chai");
const { ethers } = require("hardhat");

describe("MyNFT", function () {
  this.timeout(50000);

  let myNFT;
  let owner;
  let acc1;
  let acc2;

  this.beforeEach(async function() {
      // This is executed before each test
      // Deploying the smart contract
      const MyNFT = await ethers.getContractFactory("MyNFT");
      [owner, acc1, acc2] = await ethers.getSigners();

      myNFT = await MyNFT.deploy();
  })


});
```

In the code above we require `chai`, which is used for testing, and Hardhat. We set the timeout to **50s** because transactions may take a longer time on testnet.

The `this.beforeEach` deploys our contract before executing each test. we then obtain the account that deploys the contract, and we set this account as the owner variable.

```javascript
it("Should set the right owner", async function () {
      expect(await myNFT.owner()).to.equal(owner.address);
  });
```

The code above checks if the contract is deployed to the owner's address.

```javascript
it("Should mint one NFT", async function() {
      expect(await myNFT.balanceOf(acc1.address)).to.equal(0);
      
      const tokenURI = "https://example.com/1"
      const tx = await myNFT.connect(owner).safeMint(acc1.address, tokenURI);
      await tx.wait();

      expect(await myNFT.balanceOf(acc1.address)).to.equal(1);
  })
```

The code above checks the balance of `acc1` to be zero. It then mints one NFT to `acc1`. It waits for the transaction to complete, and it then checks if one NFT was minted to `acc1`.

Now run the command below in the root of your project directory to test the contract.

```bash
npx hardhat test
```

You will receive a response like the image below:

<img src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/test.png?raw=true">

## Deploy NFT Smart Contract

We will write a script to deploy our smart contract on the Celo Alfajores network using Hardhat. Follow the steps below to create a deployment file:

1. Navigate to the script folder under the root directory.
    
2. Open the `deploy.js` file.
    
3. Delete the code in `deploy.js`
    
4. Copy and paste the code below into `deploy.js`.
    

```javascript
const hre = require("hardhat");

async function main() {
  const MyNFT = await hre.ethers.getContractFactory("MyNFT");
  const myNFT = await MyNFT.deploy();

  await myNFT.deployed();

  console.log("MyNFT deployed to:", myNFT.address);
  storeContractData(myNFT);
}

function storeContractData(contract) {
  const fs = require("fs");
  const contractsDir = __dirname + "/contracts";

  if (!fs.existsSync(contractsDir)) {
    fs.mkdirSync(contractsDir);
  }

  fs.writeFileSync(
    contractsDir + "/MyNFT-address.json",
    JSON.stringify({ MyNFT: contract.address }, undefined, 2)
  );

  const MyNFTArtifact = artifacts.readArtifactSync("MyNFT");

  fs.writeFileSync(
    contractsDir + "/MyNFT.json",
    JSON.stringify(MyNFTArtifact, null, 2)
  );
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

Let's break down what each function in the code above do:

```javascript
const hre = require("hardhat");

async function main() {
  const MyNFT = await hre.ethers.getContractFactory("MyNFT");
  const myNFT = await MyNFT.deploy();

  await myNFT.deployed();

  console.log("MyNFT deployed to:", myNFT.address);
  storeContractData(myNFT);
}
```

The main function creates an instance of `contractFactory`. The `contractFactory` produces an instance of our contract(MyNFT). This is now deployed. We then log the contract address into the console. We then passed our deployed contract into `storeContractData` function.

```javascript
function storeContractData(contract) {
  const fs = require("fs");
  const contractsDir = __dirname + "/contracts";

  if (!fs.existsSync(contractsDir)) {
    fs.mkdirSync(contractsDir);
  }

  fs.writeFileSync(
    contractsDir + "/MyNFT-address.json",
    JSON.stringify({ MyNFT: contract.address }, undefined, 2)
  );

  const MyNFTArtifact = artifacts.readArtifactSync("MyNFT");

  fs.writeFileSync(
    contractsDir + "/MyNFT.json",
    JSON.stringify(MyNFTArtifact, null, 2)
  );
}
```

The `storeContractData` creates two JSON files that store the address and ABI of the deployed contract.

```javascript
main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

We call the main function and if an error occurs, we log the error into the console.

Save `deploy.js` and run the command below

```bash
npx hardhat run --network alfajores scripts/deploy.js
```

You will receive a response similar to the image below. Save the contract address in the response, because we will need it in the next section.

<img src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/deployed-nft.png?raw=true">

## Minting NFT

In this section, we will mint an NFT by running a Javascript file. Follow the steps below to create a javascript file that will contain the code for minting NFT.

1. Navigate to the script folder.
    
2. Create a file and name it `mint-nft.js`.
    
3. Copy the code below into `mint-nft.js`.
    
4. Save the `mint-nft.js` file
    

```javascript

const { ethers } = require("hardhat");
const contract = require("../artifacts/contracts/MyNFT.sol/MyNFT.json");
require("dotenv").config({ path: ".env" });

async function main() {
    const contractAddress = '0x50EAe049De1b4Fbe0544513633eAFE49Bd984154';

    const provider = new ethers.providers.JsonRpcProvider("https://alfajores-forno.celo-testnet.org")
    const signer = new ethers.Wallet(process.env.PRIVATE_KEY, provider)
    const abi = contract.abi
    const contractOwner = signer.address;
    const nft = new ethers.Contract(
        contractAddress,
        abi,
        signer
   );
  
    // Mint new NFTs from the collection using custom function mintCollectionNFT()
   
    const initialMintCount = 2;
    for (let i = 1; i <= initialMintCount; i++) {
        let tx = await nft.safeMint(signer.address, '1');
        await tx.wait(); // wait for this tx to finish to avoid nonce issues
        console.log(`NFT ${i} minted to ${contractOwner}`);
    }

}

main()
    .then(() => process.exit(0))
    .catch((error) => {
        console.error(error);
        process.exitCode = 1;
    });
```

The code above mints NFT into our Celo account. Let us break down what the code does.

```javascript
const { ethers } = require("hardhat");
const contract = require("../artifacts/contracts/MyNFT.sol/MyNFT.json");
require("dotenv").config({ path: ".env" });
```

* The code above imports ethers from hardhat. The `ethers` package allows us to communicate with our smart contract using Javascript code.
    
* We assign a JSON file that was generated when we deployed our code.
    
* We imported the `.env` file that we imported earlier. The `.env` file contains our Celo account private key. To get your private key, follow the steps below.
    
    * Open Metamask.
        
    * Make sure your network is set to the Celo Alfajores network.
        
    * click on the three-dot icon on the right side of the user interface.
        
    * select account details.
        
    * Enter your password.
        
    * Copy the private key shown to you.
        
    * Add the code below to the .env file.
        

```plaintext
PRIVATE_KEY='enter your private key here'
```

Now, Replace **'enter your private key here'** with the private key you copied.

```javascript
async function main() {
    const contractAddress = '0x50EAe049De1b4Fbe0544513633eAFE49Bd984154';

    const provider = new ethers.providers.JsonRpcProvider("https://alfajores-forno.celo-testnet.org")
    const signer = new ethers.Wallet(process.env.PRIVATE_KEY, provider)
    const abi = contract.abi
    const contractOwner = signer.address;
    const nft = new ethers.Contract(
        contractAddress,
        abi,
        signer
   );
}
```

In the code above the function main is an asynchronous function.

* The variable `contractAddress` contains the deployed smart contract address. Replace the contract address generated when you deployed your contract.
    
* The variable provider contains a node provider. We are using [Celo Forno](https://docs.celo.org/network/node/forno). Celo Forno allows us to communicate with the Celo blockchain without creating our node.
    
* The `signer` variable contains information about your wallet. We passed in our private key and the provider.
    
* The `abi` variable converts the contract variable into an ABI. an ABI allows us to interact with the function of our smart contract in a Javascript file.
    
* The `nft` variable contains an instance of our NFT smart contract.


Next, include the following code after the defined variables inside the `main()` function:
    
```javascript
 const initialMintCount = 2;
    for (let i = 1; i <= initialMintCount; i++) {
        let tx = await nft.safeMint(signer.address, '1');
        await tx.wait(); // wait for this tx to finish to avoid nonce issues
        console.log(`NFT ${i} minted to ${contractOwner}`);
    }
```

In the code above, we declared a `for` loop that mints two NFTs into the signer's address by calling the `safeMint` function of our smart contract.

```javascript
main()
    .then(() => process.exit(0))
    .catch((error) => {
        console.error(error);
        process.exitCode = 1;
    });
```

We called `main` function, then log possible errors encountered into the console.

Run the command below to mint your nft.

```bash
node scripts/mint-nft.js
```

After the script finishes its execution. You will get a response similar to the image below.

<img src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/mint-cmd.png?raw=true">

## Import Smart Contract.

Let's import our contract into our wallet to see the NFTs we received. Follow the steps below to import your smart contract.

1. Open the Metamask wallet.
    
2. Click on add token under asset.
    
3. Copy and paste your contract address into the field provided
    
4. Enter zero in the decimal token field.
    
5. Click on import.
    

Now, you should see your token in your wallet with at least two NFTS similar to the image below.

<img src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/mint.png?raw=true">

## Conclusion

We learned how to create, test, mint, and deploy an NFT project on the Celo blockchain network using Hardhat.
Here is a link to a [github repo](https://github.com/codeInn001/celo-hardhat) containing all the codes.

Happy coding!
