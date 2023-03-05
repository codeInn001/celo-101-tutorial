# How To Build  NFTs Using Hardhat  On Celo Blockchain

## Table of Contents
  * [Introduction](#introduction)
  * [Prerequisites](#prerequisites)
  * [Tech Stack.](#tech-stack)
  * [NFTs Smart Contract](#nfts-smart-contract)
    + [Project Setup](#project-setup)
  * [Celo Configuration](#celo-configuration)
  * [Create .env file](#create-env-file)
  * [Write and compile smart contract](#write-and-compile-smart-contract)
    + [Write Smart Contract](#write-smart-contract)
  * [Configure Metadata IPFS](#configure-metadata-ipfs)
    + [Create Image folder](#create-image-folder)
    + [Create JSON file](#create-json-file)
  * [Compile NFT Smart Contract](#compile-nft-smart-contract)
  * [Test NFT Smart Contract](#test-nft-smart-contract)
  * [Deploy NFT Smart Contract](#deploy-nft-smart-contract)
  * [Minting NFT](#minting-nft)
  * [Import Smart Contract.](#import-smart-contract)
  * [Conclusion](#conclusion)

## Introduction
<img width="1200" alt="remix-capture" src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/celo-banner.png">

Non-fungible assets are non-interchangeable assets. Non-fungible assets are also unique and different. They can not be replaced by another asset - even an asset of the same value. Examples of non-fungible assets include digital collectibles, arts, houses, domain names, etc.

We can represent non-fungible assets on the blockchain network as non-fungible tokens (NFTs). The authenticity and ownership of NFTs are easily verified on the blockchain.

In this tutorial, you will learn how to build NFTs using hardhat on the Celo blockchain network. This tutorial will enable us to:

* Build NFTs projects in our local development environment without [Remix IDE](https://remix.ethereum.org/) using hardhat.
    
* We will also deploy it on the Celo blockchain using hardhat.
    




## Prerequisites

For a proper understanding of this tutorial, you need a basic knowledge of the following:

* Basic understanding of blockchain technologies.
    
* An IDE. We will use [VS Code](https://code.visualstudio.com/download).
    
* [NFT contracts](https://hackmd.io/1_56InhcSO-oqrF8WXfhaQ).
    

## Tech Stack

In this tutorial, we will use the following tools and resources:

* [Nodejs](https://nodejs.org/). This tutorial requires version 12 or higher.
    
* Hardhat - A Ethereum development environment.
    
* [OpenZeppelin](https://www.openzeppelin.com/) Contracts - A library of secured smart contracts.
    

## NFTs Smart Contract

The Ethereum community created NFT token standard that specifies how a token behaves. This standard makes it easy for developers to build NFTs smart contracts that can interact with one another.

Celo is an EVM-compatible blockchain, Celo can use Ethereum token standard and its tools to build NFTs smart contracts.\`

Learn more about these [token standards](https://ethereum.org/en/developers/docs/standards/tokens/#:~:text=Here%20are%20some%20of%20the,for%20artwork%20or%20a%20song.).

## Project Setup

To check your node version, open your terminal, and run the command below.

```bash
node -v
```

If your node version is less than version 12, upgrade using the command below.

```bash
n latest
```

You can now verify that your node version upgrade is complete by running the command below.

```bash
node -v
```

With your terminal still opened, create a new folder by running the command below.

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

The command above will generate a package.json file. A package.json file describes the dependencies in our project. Each time you add a new dependency, **package.json** file gets an update.

With your terminal still in the **NFTs-hardhat** directory, run the command below to open the **NFTs-hardhat** directory in VSCode :

```bash
code .
```

The NFTs-hardhat directory will have the package.json file we initialized earlier. Your VS Code interface will look like the image below.
<img src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/package-json-view.png?raw=true">
Next, we need to install hardhat locally. Hardhat enables us to run, compile, debug, edit, test, and deploy our smart contract locally. Use the command below to install hardhat on your machine.

```bash
npm install --save-dev hardhat
```

After the complete installation of hardhat, your VSCode interface will have a folder structure similar to the image below.
<img src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/hardhat.png?raw=true">

To use our local installation of hardhat, run the command below. The command will initialize a sample project. We will build upon the sample project in this tutorial. The sample project will give us a good understanding of what is available in hardhat.

```bash
npx hardhat
```

The command will prompt some questions on how to set up your project. press enter key for each prompt.

After completing the initialization of the sample project, You will see an instruction in your terminal that requires that you run the command below.

```bash
 npm install --save-dev "hardhat@^2.12.7" "@nomicfoundation/hardhat-toolbox@^2.0.0"
```

The command above installs `@nomicfoundation/hardhat-toolbox` plugin that enables us to perform some actions such as:

* Deploy your smart contract.
    
* Interact with your smart contract using ether.js.
    
* Test your smart contract.
    
* Interact with hardhat network.
    

To see a list of available tasks, run the command below

```bash
npx hardhat
```

## Celo Configuration

We need to configure hardhat to enable it to deploy on Celo alfajores testnet. To do that open hardhat.config.js, located in the root folder. replace the content of hardhat.config.js with the code below.

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

In the code above, we have a [module.exports](https://www.sitepoint.com/understanding-module-exports-exports-node-js/), It contains the following properties:

* solidity: This accepts the value of our smart contract solidity version.
    
* networks: This contains the network we are deploying to, in this case, it is the alfajores testnet.
    
* alfajores: This accepts the alfajores testnet URL and accounts.
    
* accounts: This accepts the details of the Celo alfajores testnet account that we need to deploy our smart contract. The account property accepts an object with two properties. namely :
    
    1. mnemonic: This accepts a mnemonic variable from a .env file, which is required at the top of our code. This mnemonic is our wallet recovery secret phrase, it grants hardhat access to our account. This enables hardhat to interact with alfajores testnet. We will create a .env file shortly.
        
    2. path: This is also called a [derivation path](https://medium.com/mycrypto/wtf-is-a-derivation-path-c3493ca2eb52). The path provided above is a Metemask path. If you are using CeloWalletExtension, change the path to this "m/44'/52752'/0'/0".
        
* chainId: Also called a chain identifier. Accepts Celo chain id.
    

## Create .env file

Seed phrases are meant to be private. We will store our seed phrase in a .env file. The .env file prevents the upload of our seed phrase.

Run the command below to install dotenv dependency.

```bash
npm install dotenv
```

After the complete installation of dotenv, Navigate to the root directory, and create a .env file. Open your Metamask wallet, and click on identicon, located at the top right corner. Select settings, then select security and privacy. Click on Reveal Secret Recovery Phrase. Copy the phrase, and paste it into the newly created .env file. Your env file should look like the image below

```bash
MNEMONIC="YOUR_SECRET_RECOVERY_PHRASE"
```

Replace the value of the mnemonic variable with the phrase you copied from your Metamask wallet.

Now, test if hardhat is working as intended by running the command below.

```bash
npx hardhat run scripts/deploy.js --network alfajores
```

The command above deploys Lock.sol, located inside the contracts folder. You will receive a response similar to the below image in your terminal.

<img src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/deploy.png?raw=true">


## Writing the Smart Contract

In this section, we will write our smart contract using the solidity programming language. follow the steps below to create a file for our smart contract.

1. Open the contracts folder at the root of your directory
    
2. Delete the Lock.sol file.
    
3. Create a new file under the contracts folder. Name the file MyNFT.sol
    
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

On the first line is a comment indicating the [license](https://spdx.dev/) of our source file. Below the license is a `pragma` keyword that is required when we compile our code. From the above, we can see that our source file requires a compiler with a version greater than or equal to 0.8.9.

```solidity
contract MyNFT is ERC721, ERC721Enumerable, ERC721URIStorage, ERC721Burnable, Ownable
```

We created the `MyNFT` contract, which inherits the libraries imported from OpenZeppelin. These libraries contain functionalities or methods governed by [ERC721](https://eips.ethereum.org/EIPS/eip-721) standard.

The `Ownable` keyword sets access control on our smart contract, this allows only the owner to mint NFTs.

```solidity
using Counters for Counters.Counter;
Counters.Counter private _tokenIdCounter;
```

In the code above, we attach every function in Counters to Counters.Counter. It has an `increment` function that allows us to track the number of NFTs minted.

```solidity
constructor() ERC721("AfricaPrintNFT", "APNFT")
```

The constructor ERC721 has two strings passed into it. The constructor sets the two strings on deployment. It sets:

1. AfricaPrintNFT as the name of our NFT.
    
2. APNFT as the symbol of our NFT.
    

```solidity
function _baseURI() internal pure override returns (string memory) {
        return "https://ipfs.io/ipfs/QmXsvFkvY9FL3bNwx8P1BqpdzHQxUgLByo8FP5YqQ39cKN/";
    }
```

The `baseURI` returns a link that points to a JSON file that contains the metadata of our multiple tokens. This metadata includes the description, image link, and names of our tokens. If we are to mint a single token, then a `tokenURI` is enough. more on how to set your baseURI later.

```solidity
function safeMint(address to, string memory uri) public onlyOwner {
        uint256 tokenId = _tokenIdCounter.current();
        _tokenIdCounter.increment();
        _safeMint(to, tokenId);
        _setTokenURI(tokenId, uri);
    }
```

* The `safeMint` function allows the minting of tokens to the address passed to it and the URI passed to it.
    
* `string memory uri` accepts a string that points to the metadata of the to
    
* `safeMint` function contains some other methods, which it inherited from ERC721 OpenZeppelin library.
    

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

The above functions are overrides required by solidity. The functions are inherited from ERC721 OpenZeppelin library. Learn more about these functions [here](https://docs.openzeppelin.com/contracts/4.x/api/token/erc721).

## Configure Metadata IPFS

In this section, we will look at how you can configure your metadata. Metadata is a JSON file that contains information such as description, image link, and name of our tokens.

We will store the metadata and images for our token in a decentralized way using [InterPlanetary File System(IPFS)](https://docs.ipfs.tech/concepts/what-is-ipfs/) and [pinata](https://www.pinata.cloud/).

### Create Image folder

Create a folder named `img` with 3 images in the format below:

```markdown
img
├── nft_1.png
├── nft_2.png
├── nft_3.png
```

We need to upload the images above in a decentralized way using pinata to host them on IPFS. To pin your images, follow the steps below:

1. Open [pinata](https://app.pinata.cloud/) in a new tab.
    
2. Sign up. After signing up, pinata will navigate to pinata pinmanger
    
3. Click on the plus icon with add file text. Click on the upload folder, then upload the `imgfolder`.
    

After a successful upload, you will see your images with their **content identifier(CID)**. You can access these images by appending the CID of respective images to this URL- https://ipfs.io/ipfs/${CID}. Replace $CID with the CID of any of the images you wish to access.

### Create JSON file

The JSON file contains metadata for each image.

Create a folder that will contain three JSON file that represents the metadata for each image. Name its metadata in the format below:

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

Upload the metadata folder on pinata. the CID of the metadata folder on pinata will form the baseURI of our NFTs.

Replace the baseURI in MyNFT.sol with your baseURI.

## Compile NFT Smart Contract

One of the functionalities of hardhat is that we can compile our code right in our development environment. Follow the instruction below to compile your code:

1. Open your terminal
    
2. Navigate to the root directory of our project.
    
3. Run `npm install @openzeppelin/contracts`. This will install dependencies necessary for the OpenZeppelin contract we imported to run.
    
4. After the complete installation of @openzeppelin, run `npx hardhat compile`. This will compile our smart contract.
    
You will see a response similar to the image below.
    

<img src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/compile.png?raw=true">

## Test NFT Smart Contract

Let's write a test to confirm that our smart contract is working as expected. Follow the instruction below to create a file for our test:

1. Navigate to the test folder in the root directory.
2. Delete `test.js` file
3. Create a new file `nft-test.js`. Copy and paste the code below into it.
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
```

In the code above we require `chai`, which is used for testing, and hardhat. We set the timeout to 50s because transactions may take a longer time on testnet.

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

The code above checks if `acc1` has zero NFT. It then mints one NFT to `acc1`. it waits for the transaction to complete, and it then checks if one NFT was minted to `acc1`.

Now run the command below in the root of your project directory to test the contract.

```bash
npx hardhat test
```

You will receive a response like the image below:

<img src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/test.png?raw=true">

## Deploy NFT Smart Contract

We will write a script to deploy our smart contract on the Celo alfajores network using hardhat. Follow the steps below to create a deployment file:

1. Navigate to the script folder under the root directory.
    
2. Open deploy.js.
    
3. Delete the code in deploy.js
    
4. Copy and paste the code below into deploy.js.
    

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

Let's break down what each function in the code above does:

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

The main function creates an instance of contractFactory. The contractFactory produces an instance of our contract(MyNFT). This is now deployed. We then log the contract address into the console. We then passed our deployed contract into storeContractData.

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
```

The storeContractData creates a JSON file that stores the address of the deployed contract.

```javascript
main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

We call the main function and if an error occurs, we log the error into the console.

Save deploy.js and run the command below

```bash
npx hardhat run --network alfajores scripts/deploy.js
```

You will receive a response similar to the image below. Save the contract address in the response, because we will need it in the next section.

<img src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/deployed-nft.png?raw=true">

## Minting NFT

In this section, we will mint an NFT by running a javascript file. Follow the steps below to create a javascript file that will contain the code for minting NFT.

1. Navigate to the script folder.
    
2. Create a file. Name it mint-nft.js.
    
3. Copy the code below into mint-nft.js.
    
4. Save mint-nft.js file
    

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
  
    // Mint new NFTs from the collection using the custom function mintCollectionNFT()
   
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

* The code above imports ethers from hardhat. Others allow us to communicate with our smart contract in a javascript code.
    
* We assign a JSON file that was generated when we deployed our code.
    
* We imported the .env file that we imported earlier. The .env file contains our Celo account private key. To get your private key, follow the steps below.
    
    * Open Metamask.
        
    * Make sure your network is set to celo alfajores network.
        
    * click on the three-dot icon on the right side of the user interface.
        
    * select account details.
        
    * Enter your password.
        
    * Copy the private key shown to you.
        
    * Add the code below to the .env file.
        

```plaintext
PRIVATE_KEY='enter your private key here'
```

Now, Replace 'enter your private key here' with the private key you copied.

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
```

In the code above the function main is an asynchronous function.

* The variable `contractAddress` contains the deployed smart contract address. Replace the contract address generated when you deployed your contract.
    
* The variable provider contains a node provider. We are using Celo Forno. Celo Forno allows us to communicate with the Celo blockchain without creating our node.
    
* The `signer` variable contains information about your wallet. we passed in our private key and the provider.
    
* The `abi` variable converts the contract variable into an ABI. an ABI allows us to interact with the function of our smart contract in a javascript file.
    
* The `nft` variable contains an instance of our nft smart contract.
    

```javascript
 const initialMintCount = 2;
    for (let i = 1; i <= initialMintCount; i++) {
        let tx = await nft.safeMint(signer.address, '1');
        await tx.wait(); // wait for this tx to finish to avoid nonce issues
        console.log(`NFT ${i} minted to ${contractOwner}`);
    }
```

In the code above, we declared a for loop that mints two nfts into the signer's address by calling the `safeMint` function of our smart contract.

```javascript
main()
    .then(() => process.exit(0))
    .catch((error) => {
        console.error(error);
        process.exitCode = 1;
    });
```

We called the `main` function, then log possible errors encountered into the console.

Run the command below to mint your nft.

```bash
node scripts/mint-nft.js
```

After the script finishes its execution. You will get a response similar to the image below.

<img src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/mint-cmd.png?raw=true">

## Import NFT 

Let's Import our contract into our wallet to see the nfts we received. Follow the steps below to import your smart contract.

1. Open Metamask wallet.
    
2. Click on add token under asset.
    
3. Copy and paste your contract address into the field provided
    
4. Enter zero in the decimal token field.
    
5. Click on import.
    

Now, you should see your token in your wallet with at least two NFTS similar to the image below.

<img src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/mint.png?raw=true">

## Conclusion

We learned how to create, test, mint, and deploy an NFT project on the Celo blockchain network using hardhat.
Here is a link to a [github repo](https://github.com/codeInn001/celo-hardhat) containing all the codes.

Happy coding!
