# How To Build a Crowdfunding Platform Smart Contract on the Celo Blockchain Network
_Estimated reading time: **12 Minutes**_
## Introduction

<img width="1200" alt="remix-capture" src="https://github.com/codeInn001/celo-101-tutorial/blob/main/images/celo-banner.png">

Crowdfunding platforms have become popular for individuals and organizations to raise funds for projects, ideas, startups, or even charitable courses. With the emergence of blockchain technology, crowdfunding platforms can leverage blockchain technology to build a platform that is trustless, decentralized, secured, transparent, and traceable of funds and data.

In this article, we will learn how to build a decentralized crowdfunding platform on the Celo blockchain network using Solidity. Celo is a blockchain network that aims to make decentralized finance accessible to everyone. Celo reduces the barrier to crypto asset adoption by embracing a mobile-first approach.

The crowdfunding platform will enable users to donate to and create campaigns, track funds, and view a list of campaigns created.

## Table of Contents
- [How To Build a Crowdfunding Platform Smart Contract on the Celo Blockchain Network](#how-to-build-a-crowdfunding-platform-smart-contract-on-the-celo-blockchain-network)
  * [Introduction](#introduction)
  * [Table of Contents](#table-of-contents)
  * [Prerequisites](#prerequisites)
  * [Requirements](#requirements)
  * [Smart Contract Development](#smart-contract-development)
  * [Deploying the Smart Contract](#deploying-the-smart-contract)
  * [Testing the Smart Contract Using Laika](#testing-the-smart-contract-using-laika)
    + [Testing the CreateCampaign function](#testing-the-createcampaign-function)
    + [Testing getCampaigns Function](#testing-getcampaigns-function)
    + [Testing donateToCampaign Function](#testing-donatetocampaign-function)
  * [Conclusion](#conclusion)




## Prerequisites

Before you get started with this tutorial, make sure you have a basic understanding of the following:

* [Solidity](https://docs.soliditylang.org/)
    
* [Blockchain technology](https://www.simplilearn.com/tutorials/blockchain-tutorial/blockchain-technology#:~:text=advance%20your%20career.-,What%20Is%20Blockchain%20Technology%3F,computers%20participating%20in%20the%20blockchain.)
    
* [Laika](https://medium.com/laika-lab/laika-quickstart-guide-88324fdc0baa)
    
* Experience using [Remix IDE](https://remix.ethereum.org/)
    
* [How a smart contract works](https://www.ibm.com/topics/smart-contracts)
    

## Requirements

To successfully build this smart contract, your system should have the following tools:

* A chromium-based web browser
    
* An internet connection
    
* The Celo plugin activated in Remix IDE
    
* [The Remix IDE](https://remix.ethereum.org/)
    
* [The Metamask Extension Wallet](https://metamask.io/download/)
    
* A Metamask account with [Alfajores CELO tokens](https://faucet.celo.org/)
    
* A [Celo extension wallet](https://chrome.google.com/webstore/detail/celoextensionwallet/kkilomkmpmkbdnfelcpgckmpcaemjcdh?hl=en) account with Alfajores CELO tokens
    

## Smart Contract Development

Smart contract development involves creating autonomous and self-executing software programs that run on a blockchain, enabling secure and decentralized digital transactions without the need for intermediaries. In this section, we will build our smart contract using the Solidity programming language.

Follow the steps below to create a Solidity file for our crowdfunding platform:

1. Open Remix IDE in your web browser.
    
2. Create a file.
    
3. Name the file `crowdfunding.sol`.
    
4. Open the created file.
    
5. Copy the code below into the file.
    

`crowdfunding.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

contract CrowdFunding {
    struct Campaign {
        address owner;
        string title;
        string description;
        uint256 target;
        uint256 deadline;
        uint256 amountCollected;
        string image;
        address[] donators;
        uint256[] donations;
    }

    mapping(uint256 => Campaign) public campaigns;

    uint256 public numberOfCampaigns = 0;

    function createCampaign
(address _owner, string memory _title, string memory _description, uint256 _target, uint256 _deadline, string memory _image) public returns (uint256) 
{
     Campaign storage campaign = campaigns[numberOfCampaigns];

        require(campaign.deadline < block.timestamp, "The deadline should be a date in the future.");

        campaign.owner = _owner;
        campaign.title = _title;
        campaign.description = _description;
        campaign.target = _target;
        campaign.deadline = _deadline;
        campaign.amountCollected = 0;
        campaign.image = _image;

        numberOfCampaigns++;

        return numberOfCampaigns - 1;
    }

    function donateToCampaign(uint256 _id) public payable {
        uint256 amount = msg.value;

        Campaign storage campaign = campaigns[_id];

        campaign.donators.push(msg.sender);
        campaign.donations.push(amount);

        (bool sent,) = payable(campaign.owner).call{value: amount}("");

        if(sent) {
            campaign.amountCollected = campaign.amountCollected + amount;
        }
    }

    function getDonators(uint256 _id) view public returns (address[] memory, uint256[] memory) {
        return (campaigns[_id].donators, campaigns[_id].donations);
    }

    function getCampaigns() public view returns (Campaign[] memory) {
        Campaign[] memory allCampaigns = new Campaign[](numberOfCampaigns);

        for(uint i = 0; i < numberOfCampaigns; i++) {
            Campaign storage item = campaigns[i];

            allCampaigns[i] = item;
        }

        return allCampaigns;
    }
}
```

To get started, let's explain each block of code.

```solidity
// SPDX-License-Identifier: MIT
```

At the top level of our contract, we have a comment that specifies the license the contract uses. The **SPDX license identifier** is a standard format that communicates licensing information about a software package.

```solidity
pragma solidity ^0.8.9;
```

Next, we defined the version of the Solidity compiler that should be used to compile our smart contract code. In our case, we specified version 0.8.9. The **^** character means the compiler can use a version greater than 0.8.9 but less than 0.9.0. the **^** character is known as the **caret range**.

```solidity
contract CrowdFunding {
    struct Campaign {
        address owner;
        string title;
        string description;
        uint256 target;
        uint256 deadline;
        uint256 amountCollected;
        string image;
        address[] donators;
        uint256[] donations;
    }
```

In the code above, we first defined our smart contract with the keyword `contract`, then named the smart contract `CrowdFunding`.

Next, we defined a data structure with the keyword `struct` and named it `Campaign`. Within the `Campaign` struct, we declared the following variables:

* `address owner` : The address of the campaigner owner(creator).
    
* `string title` : The title of the campaign. It is given by the owner.
    
* `string description`: The description of the campaign given by the owner.
    
* `uint256 target`: The amount of funds to be raised in a campaign the owner gives.
    
* `uint256 deadline`: The date the campaign ends. It is given in the Unix time stamp.
    
* `uint256 amountCollected`: The amount of funds raised so far in the campaign
    
* `string image`: An image to upload
    
* `address[] donators`: An array of addresses of donators
    
* `uint256[] donations`: An array containing the amount donated by each donor address.
    

```solidity
mapping(uint256 => Campaign) public campaigns;
```

In the code above, we defined a mapping variable called `campaigns` that maps the `Campaign` struct to a `unit256` key. The `public` keyword means anybody can access this mapping and its value.

```solidity
uint256 public numberOfCampaigns = 0;
```

The `numberOfCampaigns` variable keeps track of the number of campaigns created.

```solidity
function createCampaign
(address _owner, string memory _title, string memory _description, uint256 _target, uint256 _deadline, string memory _image) public returns (uint256) 
{
     Campaign storage campaign = campaigns[numberOfCampaigns];

        require(campaign.deadline < block.timestamp, "The deadline should be a date in the future.");

        campaign.owner = _owner;
        campaign.title = _title;
        campaign.description = _description;
        campaign.target = _target;
        campaign.deadline = _deadline;
        campaign.amountCollected = 0;
        campaign.image = _image;

        numberOfCampaigns++;

        return numberOfCampaigns - 1;
    }
```

The `createCampaign` function creates a new campaign. It takes in the following parameters:

* `address owner`: this is the address of the campaign owner.
    
* `string memory title`: This is the title of the campaign.
    
* `string memory description`: This is the description of the campaign.
    
* `uint256 target`: This is the amount of funds to be raised.
    
* `uint256 deadline`: This is the date the campaign ends (in Unix timestamp format).
    
* `string memory image`: A URL for the image of the campaign.
    

It then creates a new `Campaign` struct and adds it to the `campaigns` mapping using the `numberOfCampaigns` variable as the key. The `storage` keyword means the variable `campaign` will be stored permanently on the blockchain so that it can be updated. It then assigns the parameters of the `createCampaign` function to the corresponding component of our struct. Finally, it increments numberOfCampaigns\` and returns the new key.

```solidity
function donateToCampaign(uint256 _id) public payable {
        uint256 amount = msg.value;

        Campaign storage campaign = campaigns[_id];

        campaign.donators.push(msg.sender);
        campaign.donations.push(amount);

        (bool sent,) = payable(campaign.owner).call{value: amount}("");

        if(sent) {
            campaign.amountCollected = campaign.amountCollected + amount;
        }
    }
```

The `donateToCampaign` function enables users to donate to a specific campaign. It accepts `_id` as a parameter. the `_id` parameter allows users to donate to a specific campaign. The function adds the **sender's address** and the **donation amount** to the **donators** and **donations** arrays in the `Campaign` struct. Then, it sends the donation amount to the campaign owner's address using the call function. If the send operation is successful, it updates the `amountCollected` in the `Campaign` struct.

```solidity
function getDonators(uint256 _id) view public returns (address[] memory, uint256[] memory) {
        return (campaigns[_id].donators, campaigns[_id].donations);
    }
```

The `getDonators` function accepts `_id` as a parameter. It then returns the donator's address and donations that correspond to the key of a specific campaign.

```solidity
function getCampaigns() public view returns (Campaign[] memory) {
        Campaign[] memory allCampaigns = new Campaign[](numberOfCampaigns);

        for(uint i = 0; i < numberOfCampaigns; i++) {
            Campaign storage item = campaigns[i];

            allCampaigns[i] = item;
        }

        return allCampaigns;
    }
```

The `getCAmpaigns` function has a `view` modifier, meaning it only reads from the smart contract and doesn't modify it. It retrieves all the campaign stored in our smart contract mapping.

The function first create an array of `Campaign` structs called `allCampaigns` with a length of `numberOfCampaigns`. This length is the total number of campaigns that have been created and stored in the `campaigns` mapping.

Then, the function uses a for loop to iterate through each campaign in the `campaigns` mapping. During each iteration, the function assigns the `Campaign` struct at the current index of the `campaigns` mapping to a new `Campaign` struct called `item`.

Finally, the function assigns the `item` struct to the corresponding index in the `allCampaigns` array.

After iterating through all campaigns in the `campaigns` mapping, the function returns the `allCampaigns` array, which contains all campaigns that have been created and stored in the `campaigns` mapping.

## Deploying the Smart Contract

Now that we understand how our smart contract works, let's deploy our smart contract on the Celo Alfajores testnet using the **Remix IDE Celo plugin**. Follow the steps below to deploy:

1. Activate the **Remix IDE Celo plugin**.
    
2. Click on the **Celo plugin icon**.
    
3. Click on **connect** button to connect **Celo extension wallet**.
    
4. Click on the **compile** button to compile the smart contract.
    
5. Click on the **deploy** button to deploy the smart contract.
    
6. Confirm the completion of the smart contract in the Celo wallet extension.
    
7. Wait for the contract to be deployed.
    
8. Take note of the generated **contract address and** [**Contract Application Binary Interface (ABI)**](https://docs.soliditylang.org/en/v0.8.19/abi-spec.html) as we will need to test our smart contract on Laika.
    

## Testing the Smart Contract Using Laika

[Laika](https://web.getlaika.app/) is a tool for quick interaction with smart contracts. Follow the steps below to test some of the functions of our smart contract.

1. Navigate to [**Laika**](https://web.getlaika.app/) in your browser.
    
2. Click on the **connect** button to connect your Metamask wallet.
    
3. Set the Metamask wallet network to Celo Alfajores.
    
4. Click on the **new requests** button close to the collections tab.
    
5. Click on the **new request** button on the interface displayed.
    
6. Copy and paste the smart contract **ABI** and **contract address** from **Remix IDE** into the provided field.
    
7. Click on the **import** button.
    

If the steps are done properly you will see a **New Contract** folder under **collections**. Inside the **New Contrac**t folder are our smart contract **functions**. Let's go ahead to test these functions.

### Testing the CreateCampaign function

Follow the steps below to create a new campaign.

1. Click on the `createCampaign` function.
    
2. Enter a reasonable value in each input field provided under the **parameter** heading.
    
3. Click on the **send** button close to the contract address input field.
    
4. Confirm the transaction in your Metamask wallet.
    

You will then receive a response at the bottom of the user interface.

If you have a problem converting the deadline date to Unix Time Stamp, use this [Unix Time Stamp converter](https://www.unixtimestamp.com/).

### Testing getCampaigns Function

Follow the steps below to create a new campaign.

1. Click on the `getCampaign` function.
    
2. Click on the **send** button close to the contract address input field.
    
3. Confirm the transaction in your Metamask wallet.
    

You will receive a response showing the details of all the campaigns created.

### Testing donateToCampaign Function

Follow the steps below to create a new campaign.

1. Click on the `donateToCampaign` function.
    
2. Enter the amount you want to donate(in CELO) in the input field beside the **send** button.
    
3. Enter the **key** of the campaign you want to donate to. The first campaign has a **key** of 0.
    
4. Click on the **send** button close to the contract address input field.
    
5. Confirm the transaction in your Metamask wallet.
    

You will receive a response showing the details of the amount donated.

## Conclusion

In this tutorial, you have learned how to implement a crowdfunding smart contract, and how to interact and test the smart contract using Laika.

Other things you can do is

* Build the front end of the smart contract.
    
* Test other functions using Laika.
    
* Add more functions such as a delete function.
    

Here is a link to the [smart contract code](https://github.com/codeInn001/celo-101-tutorial/blob/main/CrowdFunding.sol).


  
