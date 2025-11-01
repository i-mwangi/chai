# Render Environment Variables Setup Guide

## How to Add Environment Variables in Render Dashboard

1. Go to https://render.com/dashboard
2. Click on your service: `chai-platform-api`
3. Click on **"Environment"** tab (left sidebar)
4. Click **"Add Environment Variable"** button
5. Enter Key and Value
6. Click **"Save Changes"**
7. Service will automatically redeploy

---

## Required Environment Variables

Copy and paste these into Render Dashboard:

### 🔐 Critical (Database)

**Key:** `TURSO_DATABASE_URL`  
**Value:** `libsql://chai-project-i-mwangi.aws-ap-south-1.turso.io`

**Key:** `TURSO_AUTH_TOKEN`  
**Value:** `eyJhbGciOiJFZERTQSIsInR5cCI6IkpXVCJ9.eyJhIjoicnciLCJpYXQiOjE3NjE5NTQzMjcsImlkIjoiMjU5MjFhM2YtZDYyYi00MTYxLWEwYTYtNmE5OTlkZjU4OTdhIiwicmlkIjoiZWU0ZmQxNTctZmY2Ny00ZWM2LThkOWItYzc4NWI5ODYxYjQ3In0.oQMe20wVDbwcWqH2P13M8LVF7WcLeHEDuJ8QaEIr8LpAVOMsblQqhrbWmCLqBmFfYjbkWbLeucMPFo-gH_lKCA`

---

### 🔐 Critical (Hedera Account)

**Key:** `HEDERA_OPERATOR_ID`  
**Value:** `0.0.5792828`

**Key:** `HEDERA_OPERATOR_KEY`  
**Value:** `3030020100300706052b8104000a042204207ce269e5e40c72a94ce6a7daf76589e6076c110489aa866626da053315c9d326`

---

### 💰 USDC Token

**Key:** `USDC_TOKEN_ID`  
**Value:** `0.0.7144320`

**Key:** `HEDERA_USDC_TOKEN_ID`  
**Value:** `0.0.7144320`

---

### 📜 Smart Contract Addresses

**Key:** `ISSUER_CONTRACT_ID`  
**Value:** `0.0.7116112`

**Key:** `PRICE_ORACLE_CONTRACT_ID`  
**Value:** `0.0.7116062`

**Key:** `REVENUE_RESERVE_CONTRACT_ID`  
**Value:** `0.0.7116403`

**Key:** `MARKETPLACE_CONTRACT_ID`  
**Value:** `0.0.7116410`

**Key:** `FARMER_VERIFICATION_CONTRACT_ID`  
**Value:** `0.0.7116865`

**Key:** `USDC_CONTRACT_ID`  
**Value:** `0.0.7116809`

**Key:** `COFFEETREEMANAGERULTRAMINIMAL_CONTRACT_ID`  
**Value:** `0.0.7117037`

**Key:** `COFFEETREEMANAGERSIMPLIFIED_CONTRACT_ID`  
**Value:** `0.0.7116970`

**Key:** `COFFEEPRICEORACLE_CONTRACT_ID`  
**Value:** `0.0.7116996`

**Key:** `PRICEORACLE_CONTRACT_ID`  
**Value:** `0.0.7117136`

**Key:** `LENDER_CONTRACT_ID`  
**Value:** `0.0.7117419`

**Key:** `LP_USDC_TOKEN_ID`  
**Value:** `0.0.7146880`

---

### 👤 Admin Configuration

**Key:** `ADMIN_ACCOUNT_ID`  
**Value:** `0.0.6967933`

---

## Quick Copy-Paste Format

For faster entry, here's a format you can copy:

```
TURSO_DATABASE_URL=libsql://chai-project-i-mwangi.aws-ap-south-1.turso.io
TURSO_AUTH_TOKEN=eyJhbGciOiJFZERTQSIsInR5cCI6IkpXVCJ9.eyJhIjoicnciLCJpYXQiOjE3NjE5NTQzMjcsImlkIjoiMjU5MjFhM2YtZDYyYi00MTYxLWEwYTYtNmE5OTlkZjU4OTdhIiwicmlkIjoiZWU0ZmQxNTctZmY2Ny00ZWM2LThkOWItYzc4NWI5ODYxYjQ3In0.oQMe20wVDbwcWqH2P13M8LVF7WcLeHEDuJ8QaEIr8LpAVOMsblQqhrbWmCLqBmFfYjbkWbLeucMPFo-gH_lKCA
HEDERA_OPERATOR_ID=0.0.5792828
HEDERA_OPERATOR_KEY=3030020100300706052b8104000a042204207ce269e5e40c72a94ce6a7daf76589e6076c110489aa866626da053315c9d326
USDC_TOKEN_ID=0.0.7144320
HEDERA_USDC_TOKEN_ID=0.0.7144320
ISSUER_CONTRACT_ID=0.0.7116112
PRICE_ORACLE_CONTRACT_ID=0.0.7116062
REVENUE_RESERVE_CONTRACT_ID=0.0.7116403
MARKETPLACE_CONTRACT_ID=0.0.7116410
FARMER_VERIFICATION_CONTRACT_ID=0.0.7116865
USDC_CONTRACT_ID=0.0.7116809
COFFEETREEMANAGERULTRAMINIMAL_CONTRACT_ID=0.0.7117037
COFFEETREEMANAGERSIMPLIFIED_CONTRACT_ID=0.0.7116970
COFFEEPRICEORACLE_CONTRACT_ID=0.0.7116996
PRICEORACLE_CONTRACT_ID=0.0.7117136
LENDER_CONTRACT_ID=0.0.7117419
LP_USDC_TOKEN_ID=0.0.7146880
ADMIN_ACCOUNT_ID=0.0.6967933
```

---

## Variables Already in render.yaml

These are already configured and don't need to be added manually:

✅ `NODE_VERSION=18`  
✅ `API_PORT=3005`  
✅ `NODE_ENV=production`  
✅ `HEDERA_NETWORK=testnet`  
✅ `USE_HEDERA_BLOCKCHAIN=true`  
✅ `TRANSFER_TOKENS_TO_FARMER=true`  
✅ `FARMER_TOKEN_SHARE=100`  
✅ `DISABLE_INVESTOR_KYC=false`  
✅ `ENABLE_DEMO_BYPASS=false`

---

## Step-by-Step Instructions

### Method 1: Add One by One (Recommended)

1. Go to Render Dashboard
2. Click `chai-platform-api` service
3. Click "Environment" in left sidebar
4. For each variable above:
   - Click "Add Environment Variable"
   - Paste the Key (e.g., `TURSO_DATABASE_URL`)
   - Paste the Value
   - Click "Save"
5. After adding all, click "Save Changes" at bottom
6. Service will redeploy automatically (10-15 min)

### Method 2: Bulk Add (Faster)

Some Render plans allow bulk import:

1. Click "Environment" tab
2. Look for "Bulk Edit" or "Import" option
3. Paste all variables in `KEY=VALUE` format
4. Click "Save"

---

## Verification

After adding all variables and redeploying:

### 1. Check Logs
```
Render Dashboard → chai-platform-api → Logs
```

Look for:
```
✅ Hedera client initialized
✅ Connected to network: testnet
✅ Operator account: 0.0.5792828
```

### 2. Test API
```bash
curl https://chai-platform-api.onrender.com/health
```

Should show:
```json
{
  "success": true,
  "blockchain": {
    "enabled": true,
    "network": "testnet"
  }
}
```

### 3. Test Grove Registration
1. Open frontend
2. Connect wallet
3. Register grove
4. Check HashScan for transaction

---

## Security Notes

### ✅ Good Practices:
- Environment variables in Render are encrypted
- Not visible in logs
- Not exposed to frontend
- Only accessible to your service

### ⚠️ Important:
- Never commit these values to Git
- Don't share your `HEDERA_OPERATOR_KEY`
- Keep your operator account funded
- Monitor transactions on HashScan

---

## Troubleshooting

### Issue: "Environment variable not found"
**Fix:** Make sure you clicked "Save Changes" after adding all variables

### Issue: "Invalid operator key"
**Fix:** Check that the key is copied completely (it's very long!)

### Issue: "Service won't start"
**Fix:** Check logs for which variable is missing

### Issue: "Blockchain operations fail"
**Fix:** Verify operator account has HBAR balance

---

## Next Steps

1. ✅ Add all environment variables to Render Dashboard
2. ⏳ Wait for automatic redeploy (10-15 min)
3. 🧪 Test blockchain operations
4. 📊 Monitor on HashScan
5. 💰 Keep operator account funded

---

## Support

- **Render Environment Docs**: https://render.com/docs/environment-variables
- **Hedera Account**: https://hashscan.io/testnet/account/0.0.5792828
- **Turso Dashboard**: https://app.turso.tech/

---

**Total Variables to Add: 17**

Ready to add them to Render! 🚀
