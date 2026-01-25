# CoinDrop Smart Contract Deployment Guide

This guide will help you deploy the CoinDrop smart contract to the Monad blockchain.

## Prerequisites

- **Node.js** and **npm** installed
- **MetaMask** wallet with Monad network configured
- **MON tokens** for gas fees and contract funding
- **Hardhat** or **Remix** for deployment

---

## Option 1: Deploy with Remix (Easiest)

### Step 1: Open Remix
1. Go to [remix.ethereum.org](https://remix.ethereum.org)
2. Create a new file called `CoinDrop.sol`
3. Copy the contents of [CoinDrop.sol](file:///d:/New%20folder2/CoinDrop.sol) into it

### Step 2: Compile Contract
1. Click on the "Solidity Compiler" tab (left sidebar)
2. Select compiler version `0.8.19` or higher
3. Click "Compile CoinDrop.sol"
4. Ensure there are no errors

### Step 3: Deploy Contract
1. Click on "Deploy & Run Transactions" tab
2. Set Environment to "Injected Provider - MetaMask"
3. Connect your MetaMask wallet
4. Ensure you're on Monad network
5. Click "Deploy"
6. Confirm transaction in MetaMask
7. Wait for deployment confirmation

### Step 4: Fund Contract
1. After deployment, copy the contract address
2. In Remix, find the "fundContract" function
3. Enter amount (e.g., 1 MON = 1000000000000000000 wei)
4. Click "transact"
5. Confirm in MetaMask

### Step 5: Update Frontend
1. Copy the deployed contract address
2. Open [config.js](file:///d:/New%20folder2/config.js)
3. Update line 68:
   ```javascript
   contractAddress: 'YOUR_CONTRACT_ADDRESS_HERE'
   ```
4. Save the file

---

## Option 2: Deploy with Hardhat

### Step 1: Setup Hardhat Project

```bash
# Create new directory
mkdir coindrop-contract
cd coindrop-contract

# Initialize npm project
npm init -y

# Install Hardhat
npm install --save-dev hardhat

# Initialize Hardhat
npx hardhat
# Choose: Create a JavaScript project
```

### Step 2: Install Dependencies

```bash
npm install --save-dev @nomicfoundation/hardhat-toolbox
npm install dotenv
```

### Step 3: Configure Hardhat

Create `hardhat.config.js`:

```javascript
require("@nomicfoundation/hardhat-toolbox");
require("dotenv").config();

module.exports = {
  solidity: "0.8.19",
  networks: {
    monad: {
      url: process.env.MONAD_RPC_URL || "https://testnet-rpc.monad.xyz",
      accounts: [process.env.PRIVATE_KEY],
      chainId: 41454
    }
  }
};
```

### Step 4: Create Environment File

Create `.env`:

```
MONAD_RPC_URL=https://testnet-rpc.monad.xyz
PRIVATE_KEY=your_private_key_here
```

> ⚠️ **Warning**: Never commit `.env` to version control!

### Step 5: Add Contract

Copy `CoinDrop.sol` to `contracts/CoinDrop.sol`

### Step 6: Create Deployment Script

Create `scripts/deploy.js`:

```javascript
const hre = require("hardhat");

async function main() {
  console.log("Deploying CoinDrop contract...");

  const CoinDrop = await hre.ethers.getContractFactory("CoinDrop");
  const coinDrop = await CoinDrop.deploy();

  await coinDrop.deployed();

  console.log("CoinDrop deployed to:", coinDrop.address);
  
  // Fund the contract with 1 MON
  console.log("Funding contract...");
  const fundTx = await coinDrop.fundContract({
    value: hre.ethers.utils.parseEther("1.0")
  });
  await fundTx.wait();
  
  console.log("Contract funded with 1 MON");
  console.log("\nUpdate config.js with this address:");
  console.log(coinDrop.address);
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

### Step 7: Deploy

```bash
npx hardhat run scripts/deploy.js --network monad
```

### Step 8: Verify Contract (Optional)

```bash
npx hardhat verify --network monad DEPLOYED_CONTRACT_ADDRESS
```

---

## Contract Configuration

After deployment, you can configure the contract parameters:

### Set Minimum Bet
```javascript
// In Remix or via script
await contract.setMinBet(ethers.utils.parseEther("0.001"));
```

### Set Maximum Bet
```javascript
await contract.setMaxBet(ethers.utils.parseEther("1.0"));
```

### Set House Edge
```javascript
// 10% house edge
await contract.setHouseEdge(10);
```

### Check Contract Balance
```javascript
const balance = await contract.getContractBalance();
console.log("Balance:", ethers.utils.formatEther(balance), "MON");
```

---

## Testing the Contract

### Test on Monad Testnet

1. **Get Testnet Tokens**
   - Visit Monad testnet faucet
   - Request MON tokens
   - Wait for confirmation

2. **Deploy Contract**
   - Follow deployment steps above
   - Fund contract with testnet MON

3. **Test Game Flow**
   - Open the game frontend
   - Connect wallet
   - Place a bet
   - Complete game
   - Verify result on-chain

### Verify on Block Explorer

1. Go to Monad block explorer
2. Search for your contract address
3. View transactions
4. Check events (GameStarted, GameEnded)

---

## Contract Functions Reference

### Player Functions

#### placeBet(angle, force)
- **Parameters**: 
  - `angle`: 0-360
  - `force`: 30-150 (represents 0.3x-1.5x)
- **Payable**: Yes (bet amount)
- **Returns**: Game ID

#### submitResult(gameId, won, finalX, finalY)
- **Parameters**:
  - `gameId`: Game ID from placeBet
  - `won`: true/false
  - `finalX`: Final X position
  - `finalY`: Final Y position
- **Payable**: No

#### getPlayerStats()
- **Returns**: 
  - `totalGames`: Number of games played
  - `wins`: Number of wins
  - `totalWinnings`: Total winnings in wei

#### getGameResult(gameId)
- **Parameters**: `gameId`
- **Returns**: Game details

### Owner Functions

#### setMinBet(amount)
- Set minimum bet amount

#### setMaxBet(amount)
- Set maximum bet amount

#### setHouseEdge(percentage)
- Set house edge (0-50%)

#### withdraw(amount)
- Withdraw funds from contract

#### transferOwnership(newOwner)
- Transfer contract ownership

---

## Payout Calculation

The contract uses this formula:

```
Payout = BetAmount × (200 - HouseEdge) / 100
```

Examples with 10% house edge:
- Bet 0.01 MON → Win 0.018 MON (1.8x)
- Bet 0.1 MON → Win 0.18 MON (1.8x)
- Bet 1 MON → Win 1.8 MON (1.8x)

---

## Security Considerations

### Contract Security
- ✅ Reentrancy protection (checks-effects-interactions pattern)
- ✅ Owner-only functions for configuration
- ✅ Bet limits to prevent excessive losses
- ✅ Game expiration (5 minutes) to prevent stale games
- ✅ Player verification (only game creator can submit result)

### Recommendations
- 🔒 Keep private keys secure
- 🔒 Use hardware wallet for owner account
- 🔒 Start with small bet limits
- 🔒 Monitor contract balance regularly
- 🔒 Test thoroughly on testnet first

---

## Troubleshooting

### "Insufficient contract balance"
- Contract needs more funds
- Call `fundContract()` with MON

### "Game expired"
- Submit result within 5 minutes of placing bet
- Increase timeout in contract if needed

### "Not your game"
- Can only submit result for your own games
- Check you're using correct wallet

### "Game already settled"
- Result already submitted for this game
- Start a new game

---

## Mainnet Deployment Checklist

Before deploying to Monad mainnet:

- [ ] Test thoroughly on testnet
- [ ] Audit smart contract code
- [ ] Set appropriate bet limits
- [ ] Fund contract with sufficient MON
- [ ] Configure house edge
- [ ] Test all functions
- [ ] Verify contract on explorer
- [ ] Update frontend config
- [ ] Test end-to-end flow
- [ ] Monitor first few games closely

---

## Support

For issues:
1. Check contract on block explorer
2. Verify transaction status
3. Check contract balance
4. Review error messages
5. Test on testnet first

---

## License

MIT License - See contract file for details
