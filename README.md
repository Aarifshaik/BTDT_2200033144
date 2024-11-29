# BTDT_2200033144
# 💡 **Smart Utility Billing DApp** 🌐

Welcome to the **Smart Utility Billing DApp** repository! This blockchain-based project revolutionizes traditional billing systems by ensuring **transparency**, **automation**, and **security** for utility management.

---

## 🚀 **About the Project**
This Decentralized Application (DApp) automates the billing process using blockchain technology. Key features include:
- **🔗 Blockchain-based billing** to prevent tampering and ensure trust.
- **📊 Smart contract automation** for calculating bills based on consumption.
- **🌍 Local testing with Ganache** for easy proof-of-concept validation.
- **💼 Wallet integration** with MetaMask for secure transactions.

Explore how blockchain can transform utility management through this innovative DApp.

---

## 🎥 **Demonstration Videos**
Here’s everything you need to get started and understand how the project works:

1. **🦊 [MetaMask Installation](https://github.com/Aarifshaik/BTDT_2200033144/blob/main/Install%20MetaMask.mkv)**  
   Step-by-step guide to installing MetaMask for interacting with the blockchain.

2. **🍫 [Ganache Installation](https://github.com/Aarifshaik/BTDT_2200033144/blob/main/Install%20Ganache.mkv)**  
   A walkthrough for setting up Ganache, your local blockchain simulator.

3. **⚡ [Ether Transfer on Ganache](https://github.com/Aarifshaik/BTDT_2200033144/blob/main/Install%20Ganache.mkv)**  
   See how to transfer Ether between Ganache accounts using MetaMask.

4. **🌐 [Ether Transfer on Sepolia Testnet](https://github.com/Aarifshaik/BTDT_2200033144/blob/main/Install%20Ganache.mkv)**  
   Demonstrating how to perform transactions on the Sepolia test network.

5. **💻 [Local Smart Contract Execution](https://github.com/Aarifshaik/BTDT_2200033144/blob/main/Install%20Ganache.mkv)**  
   A detailed demo of executing a smart contract using Ganache.

6. **🔧 [DApp Water Bill Project Walkthrough](#)**  
   A complete guide with code walkthrough and screenshots for building the Water Bill DApp.

---

## 📄 **Project Report**
Check out the detailed [Project Report](https://github.com/Aarifshaik/BTDT_2200033144/blob/main/2200033144_AquaLedger_Project_Report.pdf) for insights into the architecture, workflow, challenges, and future scope of the DApp. 

---

## 🏗️ **Features**
- 🔒 **Secure:** All billing data is stored on an immutable blockchain ledger.
- ⚡ **Fast:** Automates bill calculation and payment processes.
- 🎨 **User-Friendly Interface:** Simple and intuitive UI for smooth interaction.
- 🌱 **Extensible:** Easily adaptable for other utilities like water or gas billing.

---

## 🛠️ **Technology Stack**
- **Frontend:** React.js
- **Smart Contracts:** Solidity
- **Blockchain Simulation:** Ganache
- **Wallet:** MetaMask

---

<!-- ## 🎯 **How to Get Started**
1. Clone this repository:  
   ```bash
   git clone https://github.com/Aarifshaik/BTDT_2200033144.git

2. **Install Dependencies**: 
   ```bash
   npm install

3. **Start Ganache**:  
   Launch Ganache to simulate a local blockchain environment.  
   
4. **Connect MetaMask**:  
   Connect your MetaMask wallet to the local blockchain network provided by Ganache.  

5. **Run the Application**:  
   Start the DApp using the following command:  
   ```bash
   npm start -->

# 🚰 Water Bill Management DApp

This project is a blockchain-based DApp for tracking water consumption and managing water bill payments securely and efficiently. It includes a smart contract (`WaterBill.sol`), deployment scripts, and a React-based frontend.

---

## 🛠️ **Setup Instructions**

### 1. **Install Prerequisites**
Ensure you have the following installed:
- [Node.js](https://nodejs.org/) (v16 or later recommended)
- [Truffle](https://www.trufflesuite.com/) (`npm install -g truffle`)
- [Ganache](https://trufflesuite.com/ganache/)
- [MetaMask](https://metamask.io/) (Browser Extension)

### 2. **Initialize the Truffle project**
   ```bash
   truffle init
   ```

### 3. **📜 Smart Contract: WaterBill.sol**
   In /contracts create WaterBill.sol:
   ```bash
   // SPDX-License-Identifier: MIT
   pragma solidity ^0.8.0;

   contract WaterBill {
      struct User {
         uint256 consumption; // in liters
         uint256 billAmount;  // in wei
      }

      mapping(address => User) public users;
      address public owner;
      uint256 public ratePerLiter; // rate in wei per liter

      event ConsumptionUpdated(address indexed user, uint256 consumption);
      event BillPaid(address indexed user, uint256 amount);
      event RateUpdated(uint256 newRate);

      modifier onlyOwner() {
         require(msg.sender == owner, "Not authorized");
         _;
      }

      constructor(uint256 _initialRate) {
         owner = msg.sender;
         ratePerLiter = _initialRate;
      }

      function updateConsumption(address _user, uint256 _consumption) public onlyOwner {
         require(_user != address(0), "Invalid address");
         users[_user].consumption = _consumption;
         users[_user].billAmount = _consumption * ratePerLiter;
         emit ConsumptionUpdated(_user, _consumption);
      }

      function getBill(address _user) public view returns (uint256) {
         return users[_user].billAmount;
      }

      function payBill() public payable {
         uint256 bill = users[msg.sender].billAmount;
         require(bill > 0, "No bill to pay");
         require(msg.value >= bill, "Insufficient payment");

         users[msg.sender].billAmount = 0;
         emit BillPaid(msg.sender, msg.value);
      }

      function setRate(uint256 _rate) public onlyOwner {
         ratePerLiter = _rate;
         emit RateUpdated(_rate);
      }

      function withdraw() public onlyOwner {
         payable(owner).transfer(address(this).balance);
      }

      receive() external payable {}
   }
   ```

### 4. **🔧 Deployment Script: 2_deploy_contracts.js**
   In migrations create 2_deploy_contracts.js:
   ```bash
   const WaterBill = artifacts.require("WaterBill");

   module.exports = function (deployer) {
   const initialRate = web3.utils.toWei("0.001", "ether"); // Initial rate: 0.001 ETH per liter
   deployer.deploy(WaterBill, initialRate);
   };
   ```

### 5. **⚙️ Truffle Configuration: truffle-config.js**
   Configure the ganache and compiler in truffle-config.js
   ```bash
   module.exports = {
   networks: {
      development: {
         host: "127.0.0.1",
         port: 8545,
         network_id: "*", // Match any network ID
      },
   },
   compilers: {
      solc: {
         version: "0.8.20", // Ensure compatibility with your Solidity version
      },
   },
   };
   ```

### 6. **📂 Interaction Script: interact.js**
Create interact.js in root of project:
   ```bash
   const WaterBill = artifacts.require("WaterBill");

   module.exports = async function (callback) {
   try {
      const instance = await WaterBill.deployed();
      const accounts = await web3.eth.getAccounts();
      const owner = accounts[0];
      const userAddress = accounts[1];

      console.log("Interacting with WaterBill contract...");

      const newRate = web3.utils.toWei("0.002", "ether");
      await instance.setRate(newRate, { from: owner });
      console.log("New rate set!");

      const consumption = 1000;
      await instance.updateConsumption(userAddress, consumption, { from: owner });
      console.log("Consumption updated!");

      const bill = await instance.getBill(userAddress);
      console.log(`Bill for ${userAddress}: ${web3.utils.fromWei(bill, "ether")} ETH`);

      await instance.payBill({ from: userAddress, value: bill });
      console.log("Bill paid!");

      await instance.withdraw({ from: owner });
      console.log("Funds withdrawn!");
      callback();
   } catch (error) {
      console.error("Error:", error);
      callback(error);
   }
   };
   ```

### 7. **Install Dependencies**
   ```bash
   npm i ganache
   ```

### 8. **Start Ganache in Terminal**
   ```bash
   ganache
   ```
   It will run at:
   Host: 127.0.0.1
   Port: 8545


### 9. **Compile**
   ```bash
   truffle compile
   ```

### 10. **Deploy**
   ```bash
   truffle migrate
   ```

### 9. **Interact**
   ```bash
   truffle exec interact.js
   ```

---

# 🚀 Post-Deployment Guide for Water Bill Management DApp

After deploying the smart contract using `truffle migrate`, follow these steps to complete the setup for your frontend and backend integration.

---

## 1. **Locate the ABI and Deployed Contract Address**

After running `truffle migrate`, the contract's **ABI** and **deployed address** will be automatically generated in the `build/contracts` directory.

### Steps:
1. #### Navigate to the generated JSON file:
   ```bash
   cd build/contracts
   ```
   Locate WaterBill.json.

2. #### Extract the ABI: Open WaterBill.json and copy the contents of the "abi" field. This is the ABI of your contract.

3. #### Extract the Deployed Address:
   - Check the console output from the truffle migrate command to find the deployed contract address.
   - Alternatively, you can programmatically retrieve it in terminal using:
   ```bash
   truffle console
   WaterBill.deployed().then(instance => console.log(instance.address));
   ```

## 2. **Frontend Project Creation**

### Steps:
1. #### Create a React project:
   ```bash
   npx create-react-app water-bill-dapp
   cd water-bill-dapp
   ```

2. #### Install dependencies:
   ```bash
   npm install web3 @mui/material @emotion/react @emotion/styled
   ```

3. #### Add the contract's ABI and deployed address to your frontend.
   Create a new file `src/WaterBillABI.js` in your React project to store the ABI and contract address.
   ```bash
   // WaterBillABI.js
   export const WaterBillABI = [
   /* Paste the ABI JSON array here */
   ];

   export const contractAddress = "0x..."; // Replace with the deployed contract address

   ```

4. #### Integrate Smart Contract in React:
   - Initialize Web3 and Contract Instance
   🌐 React Frontend: App.js:
   ```bash
   import React, { useState, useEffect } from "react";
   import Web3 from "web3";
   import { WaterBillABI, contractAddress } from "./WaterBillABI";
   import { TextField, Button, Typography, Box, Card, CardContent, CircularProgress } from "@mui/material";

   function App() {
   const [web3, setWeb3] = useState(null);
   const [accounts, setAccounts] = useState([]);
   const [contract, setContract] = useState(null);
   const [userAddress, setUserAddress] = useState("");
   const [consumption, setConsumption] = useState("");
   const [rate, setRate] = useState("");
   const [bill, setBill] = useState(null);
   const [newRate, setNewRate] = useState("");
   const [loading, setLoading] = useState(false);

   useEffect(() => {
      const init = async () => {
         try {
         // Initialize Web3
         const web3Instance = new Web3(window.ethereum);
         setWeb3(web3Instance);

         // Get user accounts
         const userAccounts = await web3Instance.eth.requestAccounts();
         setAccounts(userAccounts);

         // Set contract instance
         const contractInstance = new web3Instance.eth.Contract(WaterBillABI, contractAddress);
         setContract(contractInstance);
         } catch (error) {
         console.error("Error initializing DApp:", error);
         }
      };
      init();
   }, []);

   const handleTransaction = async (transactionFn, successMessage) => {
      setLoading(true);
      try {
         await transactionFn();
         alert(successMessage);
      } catch (error) {
         console.error("Transaction error:", error);
         alert(`Transaction failed: ${error.message}`);
      }
      setLoading(false);
   };

   return (
      <Box sx={{ padding: "20px", fontFamily: "Arial, sans-serif" }}>
         <Typography variant="h4" align="center" gutterBottom>
         Water Bill Management DApp
         </Typography>

         {/* Set New Rate */}
         <Card sx={{ marginBottom: 2 }}>
         <CardContent>
            <Typography variant="h6">Set New Rate (ETH per Liter)</Typography>
            <TextField
               label="Rate in ETH"
               variant="outlined"
               fullWidth
               margin="normal"
               value={newRate}
               onChange={(e) => setNewRate(e.target.value)}
            />
            <Button
               variant="contained"
               color="primary"
               fullWidth
               disabled={loading}
               onClick={() =>
               handleTransaction(
                  () =>
                     contract.methods
                     .setRate(web3.utils.toWei(newRate, "ether"))
                     .send({ from: accounts[1] }),
                  "Rate updated successfully!"
               )
               }
            >
               {loading ? <CircularProgress size={24} /> : "Set Rate"}
            </Button>
         </CardContent>
         </Card>

         {/* Update User Consumption */}
         <Card sx={{ marginBottom: 2 }}>
         <CardContent>
            <Typography variant="h6">Update User Consumption</Typography>
            <TextField
               label="User Address"
               variant="outlined"
               fullWidth
               margin="normal"
               value={userAddress}
               onChange={(e) => setUserAddress(e.target.value)}
            />
            <TextField
               label="Consumption (liters)"
               variant="outlined"
               fullWidth
               margin="normal"
               type="number"
               value={consumption}
               onChange={(e) => setConsumption(e.target.value)}
            />
            <Button
               variant="contained"
               color="primary"
               fullWidth
               disabled={loading}
               onClick={() =>
               handleTransaction(
                  () =>
                     contract.methods
                     .updateConsumption(userAddress, consumption)
                     .send({ from: accounts[1] }),
                  "Consumption updated successfully!"
               )
               }
            >
               {loading ? <CircularProgress size={24} /> : "Update Consumption"}
            </Button>
         </CardContent>
         </Card>

         {/* Get User Bill */}
         <Card sx={{ marginBottom: 2 }}>
         <CardContent>
            <Typography variant="h6">Get User Bill</Typography>
            <TextField
               label="User Address"
               variant="outlined"
               fullWidth
               margin="normal"
               value={userAddress}
               onChange={(e) => setUserAddress(e.target.value)}
            />
            <Button
               variant="contained"
               color="primary"
               fullWidth
               disabled={loading}
               onClick={async () => {
               setLoading(true);
               try {
                  const userBill = await contract.methods.getBill(userAddress).call();
                  setBill(web3.utils.fromWei(userBill, "ether"));
               } catch (error) {
                  console.error("Error fetching bill:", error);
                  alert(`Error fetching bill: ${error.message}`);
               }
               setLoading(false);
               }}
            >
               {loading ? <CircularProgress size={24} /> : "Get Bill"}
            </Button>
            {bill && (
               <Typography variant="body1" sx={{ marginTop: 2 }}>
               Bill: {bill} ETH
               </Typography>
            )}
         </CardContent>
         </Card>

         {/* Pay Bill */}
         <Card sx={{ marginBottom: 2 }}>
         <CardContent>
            <Typography variant="h6">Pay Bill</Typography>
            <Button
               variant="contained"
               color="secondary"
               fullWidth
               disabled={loading}
               onClick={() =>
               handleTransaction(
                  () =>
                     contract.methods
                     .payBill()
                     .send({ from: accounts[0], value: web3.utils.toWei(bill, "ether") }),
                  "Bill paid successfully!"
               )
               }
            >
               {loading ? <CircularProgress size={24} /> : "Pay Bill"}
            </Button>
         </CardContent>
         </Card>

         {/* Withdraw Funds */}
         <Card>
         <CardContent>
            <Typography variant="h6">Withdraw Funds</Typography>
            <Button
               variant="contained"
               color="success"
               fullWidth
               disabled={loading}
               onClick={() =>
               handleTransaction(
                  () => contract.methods.withdraw().send({ from: accounts[1] }),
                  "Withdrawal successful!"
               )
               }
            >
               {loading ? <CircularProgress size={24} /> : "Withdraw Funds"}
            </Button>
         </CardContent>
         </Card>
      </Box>
   );
   }

   export default App;
   ```  


5. #### Test Frontend-Backend Integration
   - Start the React app:
      ```bash
      npm start
      ```
   - Use the UI to test the following functionalities:
      - Update the water rate.
      - Update a user’s water consumption.
      - Fetch a user's bill.
      - Pay the bill.
      - Withdraw funds as Owner
---

# 🌟 Future Scope

- **📈 Historical Data**: Track past consumption and payments for better insights.  
- **🖼️ Enhanced UI/UX**: Improve the interface for a more engaging user experience.  
- **📡 Live Deployment**: Deploy the DApp on public blockchain networks like Ethereum or Polygon.  

---

## 🙌 Contributions

We welcome contributions to improve and expand this project. Here's how you can contribute:  

1. Fork this repository.  
2. Create a new branch for your feature or bug fix.  
3. Submit a pull request for review.  

---

## 📧 Contact

Have questions or feedback? Feel free to reach out:  
📩 **skaarif.10@gmail.com**  

---

## 📝 License

This project is licensed under the MIT License. See the [LICENSE](#) file for more details.  

---

🌟 **If you found this project helpful, don't forget to star the repo!** 🌟