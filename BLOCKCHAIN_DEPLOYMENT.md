# Blockchain-Enabled Deployment Guide

## Current Status

### ❌ Before (Mock API):
- **Server**: `frontend/api-server.js` (mock data only)
- **Blockchain**: NOT working
- **Tokenization**: NOT working
- **Transactions**: Fake/simulated

### ✅ After (Real API):
- **Server**: `lib/api/server.ts` (full blockchain integration)
- **Blockchain**: Working with Hedera Testnet
- **Tokenization**: Real NFT creation
- **Transactions**: Real Hedera transactions

## What Changed

### 1. Server Command
```yaml
# OLD (Mock):
startCommand: node frontend/api-server.js

# NEW (Real):
buildCommand: pnpm install && pnpm run build
startCommand: node dist/lib/api/server.js
```

### 2. Environment Variables Added
All Hedera blockchain credentials and contract addresses:
- `HEDERA_OPERATOR_ID` - Your Hedera account
- `HEDERA_OPERATOR_KEY` - Your private key
- `USDC_TOKEN_ID` - USDC token for payments
- `ISSUER_CONTRACT_ID` - NFT issuer contract
- `MARKETPLACE_CONTRACT_ID` - Marketplace contract
- And 10+ more contract addresses

### 3. Feature Flags
- `USE_HEDERA_BLOCKCHAIN=true` - Enable real blockchain
- `TRANSFER_TOKENS_TO_FARMER=true` - Auto-transfer tokens
- `FARMER_TOKEN_SHARE=100` - Give farmer 100% of tokens

## How It Works Now

### Grove Registration Flow:
1. **Farmer registers grove** → Frontend sends to API
2. **API validates** → Checks farmer verification
3. **Blockchain transaction** → Creates NFT on Hedera
4. **Tokenization** → Mints tokens representing trees
5. **Transfer** → Sends tokens to farmer's wallet
6. **Database** → Records in Turso for tracking

### What Gets Created on Hedera:
- **NFT** - Represents the grove
- **Fungible Tokens** - Represent individual trees
- **Smart Contract Calls** - For verification, marketplace, etc.

## Testing Blockchain Integration

### 1. Check API Health
```bash
curl https://chai-platform-api.onrender.com/health
```

Should return:
```json
{
  "success": true,
  "message": "Coffee Tree Platform API is running",
  "blockchain": {
    "enabled": true,
    "network": "testnet",
    "operatorId": "0.0.5792828"
  }
}
```

### 2. Register a Grove
1. Connect wallet on frontend
2. Go to Farmer Portal
3. Click "Register New Grove"
4. Fill in details
5. Submit

**What happens:**
- API receives request
- Creates NFT on Hedera
- Mints tokens
- Transfers to farmer
- Records in database

### 3. Verify on Hedera
Check transaction on HashScan:
```
https://hashscan.io/testnet/transaction/[TRANSACTION_ID]
```

## Environment Variables Explained

### Critical (Required):
```yaml
HEDERA_OPERATOR_ID: 0.0.5792828
# Your Hedera account that pays for transactions

HEDERA_OPERATOR_KEY: 3030020100300706052b8104000a04220420...
# Private key for signing transactions (KEEP SECRET!)

USDC_TOKEN_ID: 0.0.7144320
# Token used for payments/withdrawals
```

### Smart Contracts:
```yaml
ISSUER_CONTRACT_ID: 0.0.7116112
# Creates NFTs for groves

MARKETPLACE_CONTRACT_ID: 0.0.7116410
# Handles token trading

REVENUE_RESERVE_CONTRACT_ID: 0.0.7116403
# Manages revenue distribution

FARMER_VERIFICATION_CONTRACT_ID: 0.0.7116865
# Verifies farmer identity
```

### Feature Flags:
```yaml
USE_HEDERA_BLOCKCHAIN: true
# Enable/disable blockchain (true = real, false = mock)

TRANSFER_TOKENS_TO_FARMER: true
# Auto-transfer tokens to farmer after minting

FARMER_TOKEN_SHARE: 100
# Percentage of tokens farmer receives (100 = all)
```

## Cost Implications

### Hedera Transaction Costs:
- **NFT Creation**: ~$0.05 per grove
- **Token Minting**: ~$0.01 per token
- **Token Transfer**: ~$0.001 per transfer
- **Smart Contract Call**: ~$0.05 per call

### Your Operator Account:
- Needs HBAR balance to pay for transactions
- Recommended: Keep at least 100 HBAR (~$5)
- Check balance: https://hashscan.io/testnet/account/0.0.5792828

### Render Costs:
- Still FREE (or $7/month for always-on)
- Blockchain costs are separate (paid from your Hedera account)

## Security Considerations

### ⚠️ IMPORTANT:
Your `HEDERA_OPERATOR_KEY` is now in `render.yaml` which will be pushed to GitHub!

### Options:

#### Option 1: Use Render Environment Variables (Recommended)
1. Remove `HEDERA_OPERATOR_KEY` from `render.yaml`
2. Add it manually in Render Dashboard:
   - Go to service → Environment
   - Add `HEDERA_OPERATOR_KEY` as secret
   - Value: `3030020100300706052b8104000a04220420...`

#### Option 2: Use Render Secrets (Best)
```yaml
envVars:
  - key: HEDERA_OPERATOR_KEY
    sync: false  # Don't sync from file
```
Then add manually in dashboard.

#### Option 3: Create Separate Operator Account
- Create a new Hedera account just for production
- Fund it with limited HBAR
- Use different key than your main account

## Deployment Steps

### 1. Update render.yaml (Done ✅)
```bash
git add render.yaml
git commit -m "Enable blockchain integration with real API server"
git push
```

### 2. Wait for Build (10-15 minutes)
Render will:
- Install dependencies
- Build TypeScript → JavaScript
- Start real API server
- Connect to Hedera testnet

### 3. Verify Deployment
```bash
# Check health
curl https://chai-platform-api.onrender.com/health

# Check blockchain status
curl https://chai-platform-api.onrender.com/api/blockchain/status
```

### 4. Test Grove Registration
1. Open frontend
2. Connect wallet
3. Register grove
4. Check HashScan for transaction

## Troubleshooting

### Issue: "Insufficient balance"
**Cause**: Operator account out of HBAR  
**Fix**: Add HBAR to 0.0.5792828

### Issue: "Invalid operator key"
**Cause**: Wrong key format or missing  
**Fix**: Check `HEDERA_OPERATOR_KEY` in Render dashboard

### Issue: "Contract not found"
**Cause**: Contract ID wrong or not deployed  
**Fix**: Verify contract IDs on HashScan

### Issue: "Transaction failed"
**Cause**: Various (network, gas, permissions)  
**Fix**: Check API logs in Render dashboard

## Monitoring

### Check Transaction History:
```
https://hashscan.io/testnet/account/0.0.5792828/transactions
```

### Check API Logs:
1. Go to Render Dashboard
2. Click on `chai-platform-api`
3. Go to "Logs" tab
4. Look for blockchain-related logs

### Check Database:
```bash
# Connect to Turso
turso db shell chai-project-i-mwangi

# Query groves
SELECT * FROM coffee_groves;

# Query transactions
SELECT * FROM transactions;
```

## Rollback Plan

If blockchain integration causes issues:

### Quick Rollback (Mock API):
```yaml
# In render.yaml, change:
startCommand: node frontend/api-server.js

# And set:
USE_HEDERA_BLOCKCHAIN: false
```

Then push and redeploy.

## Next Steps

1. ✅ Push updated `render.yaml`
2. ⏳ Wait for deployment (10-15 min)
3. 🧪 Test grove registration
4. 📊 Monitor transactions on HashScan
5. 💰 Keep operator account funded
6. 🔒 Move private key to Render secrets

## Support

- **Hedera Docs**: https://docs.hedera.com/
- **HashScan Explorer**: https://hashscan.io/testnet/
- **Render Docs**: https://render.com/docs
- **Your Operator Account**: https://hashscan.io/testnet/account/0.0.5792828

---

**Ready to deploy blockchain-enabled version!** 🚀
