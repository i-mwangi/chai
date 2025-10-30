# ✅ CORRECT Token & Revenue Model

## The Right Way

### Tokens = Ownership (Fixed)
- Farmer owns tokens
- Investor buys tokens
- Token ownership doesn't change after purchase

### Revenue = Earnings (Distributed per harvest)
- 70% to token holders (investors)
- 30% to farmer (operational share)
- Distributed in USDC, not tokens

---

## Complete Flow

### Step 1: Grove Registration & Tokenization
```
Farmer registers grove with 1,000 trees
→ 10,000 tokens created (1,000 trees × 10 tokens/tree)
→ ALL 10,000 tokens go to FARMER ✅
→ Farmer owns 100% of tokens initially
```

### Step 2: Token Sales (Marketplace)
```
Investor A buys 1,000 tokens for $10,000 USDC
→ Farmer transfers 1,000 tokens to Investor A
→ Farmer receives $10,000 USDC
→ Farmer now has: 9,000 tokens (90%)
→ Investor A has: 1,000 tokens (10%)

Investor B buys 500 tokens for $5,000 USDC
→ Farmer transfers 500 tokens to Investor B
→ Farmer receives $5,000 USDC
→ Farmer now has: 8,500 tokens (85%)
→ Investor A has: 1,000 tokens (10%)
→ Investor B has: 500 tokens (5%)
```

### Step 3: Harvest & Revenue Distribution
```
Farmer harvests coffee and sells for $10,000 USDC

Revenue Split (70/30):
- Total Revenue: $10,000 USDC
- Investor Share (70%): $7,000 USDC
- Farmer Share (30%): $3,000 USDC

Distribution to Token Holders:
- Investor A (1,000 tokens = 10%): $700 USDC
- Investor B (500 tokens = 5%): $350 USDC
- Farmer (8,500 tokens = 85%): $5,950 USDC
- Farmer operational share: $3,000 USDC
- Farmer total: $8,950 USDC ($5,950 + $3,000)
```

---

## Key Points

1. **Tokens = Ownership** (doesn't change)
2. **Revenue = Earnings** (distributed per harvest)
3. **70/30 split applies to REVENUE, not tokens**
4. **Farmer gets ALL tokens initially**
5. **Farmer sells tokens to investors**
6. **Revenue distributed based on token ownership**

---

## Configuration

```env
# Give farmer ALL tokens (100%)
TRANSFER_TOKENS_TO_FARMER=true
FARMER_TOKEN_SHARE=100
```

This is the CORRECT model!
