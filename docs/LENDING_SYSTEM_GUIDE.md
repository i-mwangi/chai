# Lending System Guide

## Overview

The lending system allows investors to earn passive income by providing liquidity to lending pools. Farmers can borrow from these pools to fund their operations. It's similar to DeFi lending protocols like Aave or Compound.

## How It Works

### For Liquidity Providers (Investors)

#### 1. **Provide Liquidity**
You deposit USDC or other assets into a lending pool and receive LP (Liquidity Provider) tokens in return.

```
You deposit: 1,000 USDC
You receive: 1,000 LP-USDC tokens
```

#### 2. **Earn Interest**
Your deposited funds are lent out to farmers. You earn interest based on:
- **Utilization Rate**: How much of the pool is borrowed
- **APY**: Annual Percentage Yield (varies by pool)

Example:
- Pool has 150,000 USDC total
- 55,000 USDC is borrowed (37% utilization)
- Current APY: 8.5%
- Your 1,000 USDC earns: ~85 USDC per year

#### 3. **Withdraw Anytime**
You can withdraw your liquidity plus earned interest by burning your LP tokens.

```
You burn: 1,000 LP-USDC tokens
You receive: 1,085 USDC (principal + interest)
```

### For Borrowers (Farmers)

#### 1. **Request Loan**
Farmers can borrow from the pool by providing collateral (coffee grove tokens).

```
Farmer wants: 10,000 USDC
Collateral required: 12,500 USDC worth of grove tokens (125%)
Interest rate: 10% APY
```

#### 2. **Use Funds**
Borrowed funds can be used for:
- Buying equipment
- Hiring workers
- Purchasing supplies
- Expanding operations

#### 3. **Repay Loan**
Farmer repays the loan with interest. If they default, collateral is liquidated.

## Available Pools

### USDC Pool
- **Asset**: USDC Stablecoin
- **Total Liquidity**: $150,000
- **Available**: $95,000
- **Borrowed**: $55,000
- **Utilization**: 37%
- **Current APY**: 8.5%

### KES Pool
- **Asset**: Kenyan Shilling
- **Total Liquidity**: 75,000 KES
- **Available**: 45,000 KES
- **Borrowed**: 30,000 KES
- **Utilization**: 40%
- **Current APY**: 12.0%

## Step-by-Step: Providing Liquidity

### 1. Navigate to Lending Section
In the investor portal, click on "Lending" in the menu.

### 2. Choose a Pool
Review the available pools and their APYs. Higher utilization = higher APY.

### 3. Click "Provide Liquidity"
Enter the amount you want to deposit.

### 4. Confirm Transaction
- Your USDC is transferred to the pool
- You receive LP tokens (1:1 ratio)
- Transaction is recorded on Hedera blockchain

### 5. Track Your Position
View your LP token balance and accumulated interest in the "My Positions" section.

## Step-by-Step: Withdrawing Liquidity

### 1. Go to Your Positions
In the lending section, find "My Liquidity Positions".

### 2. Click "Withdraw"
Choose how many LP tokens to burn (partial or full withdrawal).

### 3. Confirm Withdrawal
- Your LP tokens are burned
- You receive USDC (principal + interest)
- Transaction recorded on blockchain

### 4. Check Your Wallet
The USDC appears in your HashPack wallet.

## How Interest is Calculated

### Utilization Rate
```
Utilization = Total Borrowed / Total Liquidity
```

Example:
```
Total Liquidity: 150,000 USDC
Total Borrowed: 55,000 USDC
Utilization: 55,000 / 150,000 = 36.7%
```

### APY (Annual Percentage Yield)
APY increases with utilization to incentivize more liquidity when demand is high.

```
Base APY: 5%
Utilization Multiplier: 1 + (Utilization × 2)
Current APY: 5% × (1 + 0.367 × 2) = 8.67%
```

### Your Earnings
```
Your Deposit: 1,000 USDC
APY: 8.5%
Time: 1 year
Earnings: 1,000 × 0.085 = 85 USDC
```

For shorter periods:
```
Daily: 85 / 365 = 0.23 USDC per day
Monthly: 85 / 12 = 7.08 USDC per month
```

## LP Tokens Explained

### What are LP Tokens?
LP (Liquidity Provider) tokens represent your share of the lending pool.

### How They Work
- **1:1 Ratio**: 1 USDC deposited = 1 LP-USDC token
- **Transferable**: You can trade or sell LP tokens
- **Redeemable**: Burn LP tokens to get your USDC back
- **Interest-Bearing**: Value increases over time as interest accrues

### Example
```
Day 1: Deposit 1,000 USDC → Receive 1,000 LP-USDC
Day 365: Burn 1,000 LP-USDC → Receive 1,085 USDC
```

The LP token itself doesn't change quantity, but its redemption value increases.

## Risks

### 1. **Smart Contract Risk**
Bugs in the lending contract could result in loss of funds.

**Mitigation**: Contracts are audited and tested.

### 2. **Liquidity Risk**
If utilization is 100%, you can't withdraw until someone repays.

**Mitigation**: Pools maintain reserve ratios.

### 3. **Default Risk**
If borrowers default and collateral value drops, the pool loses money.

**Mitigation**: Over-collateralization (125%+) and liquidation mechanisms.

### 4. **Interest Rate Risk**
APY fluctuates based on utilization.

**Mitigation**: Diversify across multiple pools.

## Current Implementation Status

### ✅ Implemented
- Lending pool display
- Provide liquidity functionality
- Withdraw liquidity functionality
- LP token minting/burning
- Interest calculation
- Transaction history
- Real USDC transfers on Hedera

### 🚧 In Development
- Borrowing functionality (farmers)
- Collateral management
- Liquidation system
- Dynamic APY calculation
- Multi-asset support

### 📋 Planned
- Flash loans
- Governance tokens
- Insurance fund
- Cross-chain bridges

## API Endpoints

### Get Lending Pools
```
GET /api/lending/pools
Response: { success: true, pools: [...] }
```

### Provide Liquidity
```
POST /api/lending/provide-liquidity
Body: {
  assetAddress: "USDC",
  amount: 1000,
  providerAddress: "0.0.123456"
}
```

### Withdraw Liquidity
```
POST /api/lending/withdraw-liquidity
Body: {
  assetAddress: "USDC",
  lpTokenAmount: 1000,
  providerAddress: "0.0.123456"
}
```

### Get Pool Statistics
```
GET /api/lending/pool-statistics?assetAddress=USDC
Response: { success: true, pool: {...} }
```

## Frontend Components

### Lending Pools Display
Shows all available pools with:
- Asset name
- Total liquidity
- Utilization rate
- Current APY
- Available to borrow

### Provide Liquidity Modal
Form to deposit funds:
- Amount input
- Pool selection
- Transaction confirmation

### My Positions
Shows your LP token holdings:
- Pool name
- LP tokens owned
- Current value
- Earned interest
- Withdraw button

### Withdrawal Modal
Form to withdraw funds:
- LP token amount
- Expected USDC return
- Transaction confirmation

## Example User Flow

### Sarah's Lending Journey

**Day 1: Provide Liquidity**
```
Sarah has: 5,000 USDC
She deposits: 5,000 USDC into USDC pool
She receives: 5,000 LP-USDC tokens
Pool APY: 8.5%
```

**Day 180: Check Earnings**
```
Time passed: 6 months
Interest earned: 5,000 × 0.085 × 0.5 = 212.50 USDC
Current value: 5,212.50 USDC
LP tokens: Still 5,000 (quantity doesn't change)
```

**Day 365: Withdraw**
```
Sarah burns: 5,000 LP-USDC tokens
She receives: 5,425 USDC (principal + full year interest)
Profit: 425 USDC (8.5% return)
```

## Tips for Maximizing Returns

### 1. **Choose High-Utilization Pools**
Higher utilization = higher APY

### 2. **Compound Your Interest**
Withdraw interest and re-deposit to compound.

### 3. **Diversify Across Pools**
Don't put all funds in one pool.

### 4. **Monitor Utilization**
If utilization hits 100%, you can't withdraw. Watch the metrics.

### 5. **Long-Term Holding**
Interest compounds over time. Longer deposits = better returns.

## Troubleshooting

### "Insufficient liquidity available"
The pool doesn't have enough available funds. Wait for borrowers to repay or choose another pool.

### "Transaction failed"
Check that you have:
- Enough USDC in your wallet
- Sufficient HBAR for transaction fees
- Token association for LP tokens

### "LP tokens not showing in wallet"
You need to associate the LP token in HashPack first (similar to grove tokens).

## Related Documentation

- Token Association Guide: `docs/TOKEN_ASSOCIATION_GUIDE.md`
- Withdrawal Service: `api/withdrawal-service.ts`
- Lending API: `api/lending-api.ts`
- Hedera Lending Service: `api/hedera-lending-service.ts`

## Future Enhancements

### Phase 1: Core Lending (Current)
- ✅ Liquidity provision
- ✅ Withdrawals
- ✅ Interest calculation

### Phase 2: Borrowing
- 🚧 Loan requests
- 🚧 Collateral management
- 🚧 Repayment system

### Phase 3: Advanced Features
- 📋 Flash loans
- 📋 Liquidations
- 📋 Governance

### Phase 4: DeFi Integration
- 📋 Cross-chain bridges
- 📋 Yield aggregation
- 📋 Insurance protocols
