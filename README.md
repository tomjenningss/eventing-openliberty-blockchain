# Listen to events out of a distributed blockchain network using Open Liberty


> 2 Org Hyperledger Fabric sample using Open Liberty to execute transactions and listen to events with IBM Blockchain Platform


In this tutorial, you will learn to:

* Use the IBM Blockchain Platform extension to create a fully distributed 2 Org local blockchain network and deploy a sample smart contract (based on cars)
* Use the [Open Liberty](https://openliberty.io) extension to start two web servers that can communicate with your blockchain network
* Transact on the blockchain network from one web server via REST APIs to sell cars and listen for events emitted out of Hyperledger fabric on the buyer organization. 

Learn about the fundamentals of blockchain and Open Liberty by following the [Integrate Java microservices with blockchain using Hyperledger Fabric and Open Liberty](https://developer.ibm.com/tutorials/integrate-java-microservices-with-blockchain-using-hyperledger-fabric-and-open-liberty/) to experience starting a 1 Org blockchain network and an Open Liberty server to execute transactions to blockchain. Also, follow this to follow the instructions on installing the IBM Blockchain platform VS Code extension.

You will be able to use a fully distributed 2 Org blockchain network. Leveraging a distributed network and Open Liberty to submit transactions and listen to events.

Using blockchain provides supply chains a permanent record of transactions which are grouped in blocks that cannot be altered, creating an alternative to traditional paper tracking and manual inspection systems, which can leave supply chains vulnerable to inaccuracies and fraud.

In our example we are creating a decentralized blockchain network using a buyer and a seller in a supply chain for car sales, based on the fabcar sample.

The seller (Org 1) can add a car to the blockchain network, which will emit an event notifying other organisations an event has occured on the blockchain.
A buyer (Org 2) can become notified that a new car has been added to the blockchain network. The buyer will be able to see the new car which is up for sale. If the buyer is looking for a new car, they can query the new car which has been added to the blockchain network. 

## Architecture flow

 <img src="images/ArchitectDiagram.jpg" alt="drawing">

## Prerequisites:

* Java
* Git
* Maven
* Docker
* VS Code

## Steps

* Import the Open Liberty projects into VS Code

* Import the "fabcar" sample smart contract project into VS Code

* Start a 2 Org blockchain network and deploy the contract to both organisations

* Export credentials for Org1 and Org2 to communicate with the blockchain network

* Startup the buyer and seller Open Liberty servers.

* Add a car to the ledger from Org1.

* Look at the events out of hyperledger fabric.

* Update the owner of a Car on the Ledger

* Optional - View Open Liberty Metrics

* Stop the Open Liberty server

* Stop the Blockchain Network

* Finished



## 1. Import the Open Liberty projects into VS Code

1. Add the Org1 project to VS Code, select **File** > **Open** > **eventing/org1**, and then click **Open**.

    This will add the Org1 project to the workspace and will automatically add `Liberty Dev Dashboard` into the VS Code extension. Clicking on the `Liberty Dev Dashboard` will display `org1-ol-blockchain`

1. Open a new VS Code window to add the Org2 project **File** > **New Window**. Import the Org2 project **File** > **Open** > **eventing/org2**, and then click **Open**.

    Adding the Org2 project is what makes the blockchain network dectralized as it will add multiple organizations. Open the `Liberty Dev Dashboard` to view the `org2-ol-blockchain`.

## 2. Import the FabCar sample smart contract project into VS Code

1. Click the IBM Blockchain Platform icon in the top right corner (looks like a square).

   <img src="images/blockchainlogo.png" alt="drawing">

    It may take a moment. In the purple bar at the bottom, it will say, "Activating extension."

1. Select **FabCar** from the "Explore sample code" section.

1. Click the **Clone** button to git clone the sample code for the FabCar sample, and choose a convenient location to clone the fabric sample.

1. Select **Clone**. 

   <img src="images/Fabcarsample-repo.png" alt="drawing">

1.  From the list of options choose, **FabCar v1.0.0 Java**.

1. Click **Open Locally**.

   <img src="images/openlocally.png" alt="drawing">

1. In the Command Palette, click **Add to workspace**.

1. *Optional*: Click the **File explorer** button in the top left, and you will see `fabcar-contract-java`, which is the project to create the blockchain network.

1. Click the IBM Blockchain Platform icon on the left side to navigate back to the IBM Blockchain Platform extension for VS Code.

## 4. Start the 2 Org blockchain network and deploy the contract

1. Within **FABRIC ENVIRONMENTS**, select the ` + ` icon to create a customized blockchain network.

1. Select `Create  new from template`, using a template blockchain network structure created by IBM Blockchain platform team

1. As we are creating a **2 Org network** select **2 Org template (2CAs, 2 peers, 1 channel)**

    <img src="images/2OrgTemplate.png" alt="drawing">

1. Name the Environment `2 Org Local Fabric`

1. Once you're connected to the "2 Org Local Fabric" environment (this happens automatically after it has started), under **Smart Contracts** > **Instantiated**, click **+Instantiate**.`

1. Choose **fabcar-contract-java Open Project** (at the Command Palette prompt).

1. When prompted to `Enter a name for your Java Package`, enter `fabcar`, and press **Enter**.

1. When prompted to `Enter a version for your Java package`, enter `2.0.0`.

1. Select all the Peers to install the smart contract onto and press **OK**
    
    In a supply chain environment the smart contract is a self-enforcing agreement between parties. If all parties agree who are bound into that smart contract data can be added to the blockchain.

    In our example the buyer and the seller are both agreeing to the terms and conditions of the smart contract. 

  <img src="images/all-peer-installation.png" alt="drawing">

  1. When "Optional functions" appears, enter `initLedger`. This initializes the ledger with unique cars. Not entering the function will result in the blockchain network being empty.

   <img src="images/initLedger-function.png" alt="drawing">

1. For all other "Optional functions", press **Enter** to skip.

1. When asked, `Do you want to provide a private data collection configuration file?`, select `No`, as you do not need any private data configuration files.

The notification window at the bottom left will say, `IBM Blockchain Platform extension: Instantiating Smart Contract`. It will take approximately 2 - 5 minutes to instantiate the smart contract.

## 5. Export credentials to communicate with the blockchain network

For Open Liberty to communicate to the blockchain network, Hyperledger Fabric has security features that stop applications attempting to make transactions unless you have the specific profiles and certificate authorities. 

1. Export the Local Fabric Gateways:

   1. In the "FABRIC GATEWAYS" panel, select `2 Org Local Fabric`.

        As there are multiple organizations, we need to connect to both `Org1` and `Org2` as admin to export the connection profiles.  

   1. Select `Org1` and "Choose an identity to connect with" will appear from the command palette. Select **admin**.

      <img src="images/identity-admin.png" alt="drawing" width="500" height="180">

   1. Hover over the **FABRIC GATEWAYS** heading, click **...** > **Export connection profile**.

      <img src="images/export-fab-gateway.png" alt="drawing">

   1. The `finder` window will open.

   1. Navigate to `Users/Shared/`.

   1. Create a new folder `FabConnection`.

      The full path directory should be `Users/Shared/FabConnection`.

   1. Save the `.json` file as `2-Org-Local-Fabric-Org1_connection.json`.

   1. To disconnect from Org1 Fabric gateway press the "door" icon 

    <img src="images/exit-door.png" alt="drawing">

  1. 


1. Export the Fabric Wallets:

   1. In the "FABRIC WALLETS" panel, select **1 Org Local Fabric**, then right-click **Org1**, and select **Export Wallet**.

      <img src="images/export-org1.png" alt="drawing">

   1. Save the folder as `wallet` in the `/Users/Shared/FabConnection/` directory.

