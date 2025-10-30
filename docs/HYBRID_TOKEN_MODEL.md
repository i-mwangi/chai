# Hybrid Token Distribution Model

## Overview

The hybrid model combines the benefits of centralized and decentralized token management:
- **Initial Phase**: Platform holds all tokens (easy management)
- **Distribution Phase**: Transfer tokens to farmers (true ownership)

---

## How It Works

### Phase 1: Token Creation (Centralized)
```
Farmer registers grove
→ Token created with 100,000 tokens
→ All tokens go to platform account (0.0.5792828)
→ Platform manages token sales
```

### Phase 2: Investor Sales (Managed)
```
Investors buy tokens from marketplace
→ Platform transfers tokens to investors
→ Platform collects USDC payments
→ Platform holds USDC for farmer
```

### Phase 3: Farmer Distribution (Decentralized)
```
After sales complete (or at any time)
→ Platform transfers remaining tokens to farmer
→ Farmer now owns and controls their tokens
→ Farmer can sell on secondary market
```

---

## Configuration

### Automatic Transfer (Optional)

Edit `.env` file:
```env
# Automatically transfer tokens to farmer after tokenization
TRANSFER_TOKENS_TO_FARMER=true

# Percentage of tokens to give farmer (default: 30%)
FARMER_TOKEN_SHARE=30
```

**When enabled:**
- 30% of tokens → Farmer's wallet (immediately)
- 70% of tokens → Platform treasury (for investor sales)

**When disabled (default):**
- 100% of tokens → Platform treasury
- Manual transfer later using CLI script

---

## Manual Transfer

### Transfer to Specific Grove

```bash
# Transfer 30% of tokens to farmer for grove ID 23
npm run transfer-tokens -- --grove-id=23

# Transfer custom percentage (e.g., 40%)
npm run transfer-tokens -- --grove-id=23 --percentage=40
```

### Transfer to All Groves

```bash
# Transfer 30% to all farmers
npm run transfer-tokens -- --all

# Transfer custom percentage to all
npm run transfer-tokens -- --all --percentage=50
```

---

## Use Cases

### Use Case 1: Initial Launch
```
Strategy: Keep all tokens in platform
Reason: Easier to manage initial sales
Action: Set TRANSFER_TOKENS_TO_FARMER=false

Timeline:
1. Month 1-3: Platform sells 70% of tokens
2. Month 4: Transfer remaining 30% to farmers
3. Ongoing: Farmers manage their tokens
```

### Use Case 2: Farmer Incentive
```
Strategy: Give farmers immediate ownership
Reason: Build trust, show transparency
Action: Set TRANSFER_TOKENS_TO_FARMER=true, FARMER_TOKEN_SHARE=20

Result:
- 20% tokens → Farmer (immediate)
- 80% tokens → Platform (for sales)
- Farmer sees tokens in wallet immediately
```

### Use Case 3: Gradual Transition
```
Strategy: Transfer tokens over time
Reason: Smooth transition to decentralization
Action: Manual transfers at milestones

Timeline:
1. Launch: 100% platform
2. 25% sold: Transfer 10% to farmer
3. 50% sold: Transfer another 15% to farmer
4. 75% sold: Transfer remaining to farmer
```

---

## Example Scenarios

### Scenario A: 10,000 Token Grove

**Initial State:**
```
Total Tokens: 10,000
Platform: 10,000 (100%)
Farmer: 0 (0%)
```

**After 70% Sales:**
```
Total Tokens: 10,000
Investors: 7,000 (70%)
Platform: 3,000 (30%)
Farmer: 0 (0%)
```

**After Transfer to Farmer:**
```
Total Tokens: 10,000
Investors: 7,000 (70%)
Platform: 0 (0%)
Farmer: 3,000 (30%) ✅
```

### Scenario B: With Automatic Transfer (30%)

**Initial State (After Tokenization):**
```
Total Tokens: 10,000
Platform: 7,000 (70%)
Farmer: 3,000 (30%) ✅ Immediate
```

**After 70% Sales:**
```
Total Tokens: 10,000
Investors: 7,000 (70%)
Platform: 0 (0%)
Farmer: 3,000 (30%)
```

---

## Benefits

### For Platform
✅ Control initial token distribution  
✅ Manage marketplace liquidity  
✅ Easier investor onboarding  
✅ Flexible distribution strategy  

### For Farmers
✅ Eventually own their tokens  
✅ Can see tokens in wallet  
✅ Control secondary sales  
✅ True ownership achieved  

### For Investors
✅ Easy token purchase process  
✅ Platform-managed liquidity  
✅ Transparent token distribution  
✅ Secondary market opportunities  

---

## API Usage

### Programmatic Transfer

```typescript
import { transferTokensToFarmer } from './api/transfer-tokens-to-farmer'

// Transfer 30% of tokens to farmer
const result = await transferTokensToFarmer(groveId, 30)

if (result.success) {
    console.log(`Transferred ${result.tokensTransferred} tokens`)
} else {
    console.error(`Transfer failed: ${result.error}`)
}
```

### Batch Transfer

```typescript
import { batchTransferToFarmers } from './api/transfer-tokens-to-farmer'

// Transfer to multiple groves
const groveIds = [23, 24, 25]
const results = await batchTransferToFarmers(groveIds, 30)

console.log(`Successful: ${results.filter(r => r.success).length}`)
```

### Check Groves Needing Transfer

```typescript
import { getGrovesNeedingTransfer } from './api/transfer-tokens-to-farmer'

const groves = await getGrovesNeedingTransfer()
console.log(`${groves.length} groves need token transfer`)
```

---

## Best Practices

### 1. Start Centralized
- Keep tokens in platform initially
- Build trust with successful sales
- Transfer tokens after proving the model

### 2. Communicate Clearly
- Tell farmers tokens will be transferred
- Set expectations on timeline
- Show transparency in process

### 3. Transfer Strategically
- Transfer after significant sales (e.g., 50-70%)
- Keep some tokens for liquidity
- Consider farmer's technical ability

### 4. Document Everything
- Record all transfers
- Provide transaction IDs
- Update farmer dashboard

---

## Monitoring

### Check Token Balances

```bash
# View token distribution for a grove
npm run check-token-balance -- --grove-id=23
```

### View Transfer History

```bash
# See all token transfers
npm run transfer-history -- --grove-id=23
```

---

## Troubleshooting

### Transfer Failed: Insufficient Balance
**Problem**: Platform doesn't have enough tokens  
**Solution**: Check if tokens were already sold/transferred

### Transfer Failed: Invalid Signature
**Problem**: Platform key doesn't have permission  
**Solution**: Ensure platform is token admin/supply key

### Farmer Can't See Tokens
**Problem**: Farmer hasn't associated token  
**Solution**: Farmer must associate token in HashPack first

---

## Summary

The hybrid model gives you:
- **Flexibility**: Choose when to transfer tokens
- **Control**: Manage initial distribution
- **Decentralization**: Eventually give farmers ownership
- **Trust**: Show commitment to farmer ownership

**Recommended Strategy:**
1. Start with platform holding all tokens
2. Sell 60-70% to investors
3. Transfer remaining 30-40% to farmers
4. Farmers manage secondary sales

This balances ease of use with true decentralization!
