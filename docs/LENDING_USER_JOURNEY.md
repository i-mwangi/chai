# Lending System - Complete User Journey Guide

## Overview

This guide walks through every step of using the lending system, showing what happens behind the scenes and what you'll see on the frontend.

---

## Scenario 1: You Provide Liquidity (Become a Lender)

### Step 1: First Time Setup - Associate LP Token

**What You Do:**
1. Open HashPack wallet
2. Go to "Tokens" tab
3. Click "Associate Token"
4. Enter LP token ID: `0.0.7146880`
5. Confirm transaction (costs ~$0.05 HBAR)

**What Happens:**
- Your Hedera account is now linked to the LP-USDC token
- You can now receive LP tokens
- This is a one-time setup

**Frontend Display:**
```
┌─────────────────────────────────────┐
│  ✅ Token Associated Successfully   │
│                                     │
│  You can now provide liquidity to   │
│  the USDC pool                      │
└─────────────────────────────────────┘
```

---

### Step 2: Provide Liquidity

**What You Do:**
1. Go to Lending section in investor portal
2. See available pools:
   ```
   ┌─────────────────────────────────────┐
   │  USDC Pool                          │
   │  Total Liquidity: $150,000          │
   │  Available: $95,000                 │
   │  Borrowed: $55,000                  │
   │  Utilization: 37%                   │
   │  Current APY: 8.5%                  │
   │                                     │
   │  [Provide Liquidity]                │
   └─────────────────────────────────────┘
   ```
3. Click "Provide Liquidity"
4. Enter amount: `1000` USDC
5. Click Submit

**What Happens Behind the Scenes:**

```
Frontend → Backend API
    ↓
Backend checks: Is user associated with LP token?
    ✅ Yes
    ↓
Backend mints 1,000 LP-USDC tokens
    Transaction: 0.0.5792828@1234567890.123456789
    ↓
Backend transfers 1,000 LP-USDC to your wallet
    Transaction: 0.0.5792828@1234567890.987654321
    ↓
Backend records in database:
    - Position ID: lp_1234567890_967933
    - Asset: USDC
    - Amount: 1000
    - Account: 0.0.6967933
    - Timestamp: 1234567890
    ↓
Success!
```

**Frontend Display:**

Loading state:
```
┌─────────────────────────────────────┐
│  ⏳ Processing liquidity provision  │
│                                     │
│  Please wait...                     │
└─────────────────────────────────────┘
```

Success:
```
┌─────────────────────────────────────┐
│  ✅ Liquidity Provided Successfully │
│                                     │
│  Amount: 1,000 USDC                 │
│  LP Tokens Received: 1,000 LP-USDC  │
│                                     │
│  Transaction:                       │
│  0.0.5792828@1234567890.987654321   │
│                                     │
│  [View on HashScan]                 │
└─────────────────────────────────────┘
```

**What You See in HashPack:**
```
Tokens:
  HBAR: 98.50 ℏ
  USDC: 5,000.00 (unchanged - no actual deposit yet)
  LP-USDC: 1,000.00 ← NEW!
```

---

### Step 3: View Your Position

**Frontend Display:**

Navigate to "My Positions" section:

```
┌─────────────────────────────────────────────────┐
│  My Liquidity Positions                         │
├─────────────────────────────────────────────────┤
│                                                 │
│  ┌───────────────────────────────────────────┐ │
│  │  USDC Pool              $1,000.00  +$0.00 │ │
│  │  ─────────────────────────────────────────│ │
│  │  LP Tokens:           1,000.00            │ │
│  │  Pool Share:          0.67%               │ │
│  │  Initial Investment:  $1,000.00           │ │
│  │  Current APY:         12.0%               │ │
│  │  Provided Date:       Jan 27, 2025        │ │
│  │                                           │ │
│  │  [Withdraw Liquidity]                     │ │
│  └───────────────────────────────────────────┘ │
│                                                 │
└─────────────────────────────────────────────────┘
```

**What This Means:**
- You have 1,000 LP-USDC tokens
- They represent your share of the pool
- You're earning 12% APY (mock - not real yet)
- You can withdraw anytime

---

## Scenario 2: Someone Borrows from the Pool

### Current Implementation (Mock)

**What Happens:**
```
Farmer requests loan:
    Amount: 10,000 USDC
    Collateral: Grove tokens worth $12,500
    Interest Rate: 10% APY
    ↓
Backend creates loan record (in-memory)
    ↓
Mock pool data updated:
    Available Liquidity: $95,000 → $85,000
    Total Borrowed: $55,000 → $65,000
    Utilization: 37% → 43%
    ↓
APY increases (mock calculation):
    Your APY: 12% → 13.2%
```

**Frontend Display:**

Pool stats update:
```
┌─────────────────────────────────────┐
│  USDC Pool                          │
│  Total Liquidity: $150,000          │
│  Available: $85,000 ↓               │
│  Borrowed: $65,000 ↑                │
│  Utilization: 43% ↑                 │
│  Current APY: 13.2% ↑               │
└─────────────────────────────────────┘
```

Your position updates:
```
┌───────────────────────────────────────────┐
│  USDC Pool              $1,000.00  +$0.00 │
│  ─────────────────────────────────────────│
│  LP Tokens:           1,000.00            │
│  Pool Share:          0.67%               │
│  Initial Investment:  $1,000.00           │
│  Current APY:         13.2% ↑             │
│  Provided Date:       Jan 27, 2025        │
└───────────────────────────────────────────┘
```

**Important:** 
- ⚠️ Borrowing is NOT actually implemented yet
- This is just mock data for demonstration
- No real USDC is lent out
- No real interest is earned

---

## Scenario 3: You Withdraw Your Liquidity

### Step 1: Initiate Withdrawal

**What You Do:**
1. Go to "My Positions"
2. Find your USDC Pool position
3. Click "Withdraw Liquidity"
4. Modal appears:

```
┌─────────────────────────────────────────────────┐
│  Withdraw Liquidity                             │
├─────────────────────────────────────────────────┤
│                                                 │
│  Available LP Tokens: 1,000.00                  │
│                                                 │
│  Amount to Withdraw:                            │
│  ┌─────────────────────────────────────────┐   │
│  │ 1000                                    │   │
│  └─────────────────────────────────────────┘   │
│                                                 │
│  You will receive:                              │
│  • Principal: 1,000.00 USDC                     │
│  • Interest: 50.00 USDC (5%)                    │
│  • Total: 1,050.00 USDC                         │
│                                                 │
│  [Cancel]  [Confirm Withdrawal]                 │
└─────────────────────────────────────────────────┘
```

5. Click "Confirm Withdrawal"

**What Happens Behind the Scenes:**

```
Frontend → Backend API
    ↓
Backend checks: Does user have 1,000 LP tokens?
    ✅ Yes (checks Hedera balance)
    ↓
Backend calculates return:
    Principal: 1,000 USDC
    Interest: 50 USDC (5% mock)
    Total: 1,050 USDC
    ↓
Backend burns 1,000 LP-USDC tokens
    Transaction: 0.0.5792828@1234567891.123456789
    ↓
Backend transfers 1,050 USDC to your wallet
    Transaction: 0.0.5792828@1234567891.987654321
    ↓
Backend records withdrawal in database:
    - Withdrawal ID: lw_1234567891_967933
    - Asset: USDC
    - Amount: 1050
    - Account: 0.0.6967933
    - Timestamp: 1234567891
    ↓
Success!
```

**Frontend Display:**

Loading:
```
┌─────────────────────────────────────┐
│  ⏳ Processing withdrawal...        │
│                                     │
│  Burning LP tokens...               │
│  Transferring USDC...               │
└─────────────────────────────────────┘
```

Success:
```
┌─────────────────────────────────────┐
│  ✅ Withdrawal Successful!          │
│                                     │
│  LP Tokens Burned: 1,000.00         │
│  USDC Received: 1,050.00            │
│  • Principal: 1,000.00              │
│  • Interest: 50.00                  │
│                                     │
│  Transactions:                      │
│  Burn: 0.0.5792828@...123456789     │
│  Transfer: 0.0.5792828@...987654321 │
│                                     │
│  [View on HashScan]                 │
└─────────────────────────────────────┘
```

**What You See in HashPack:**
```
Tokens:
  HBAR: 98.45 ℏ (slightly less from fees)
  USDC: 6,050.00 ↑ (was 5,000, now +1,050)
  LP-USDC: 0.00 ↓ (was 1,000, now burned)
```

---

## Scenario 4: Partial Withdrawal

### What You Do:
1. Go to "My Positions"
2. Click "Withdraw Liquidity"
3. Enter amount: `500` (instead of full 1,000)
4. Confirm

**What Happens:**
```
Backend burns 500 LP tokens
    ↓
Backend calculates return:
    Principal: 500 USDC
    Interest: 25 USDC (5%)
    Total: 525 USDC
    ↓
Backend transfers 525 USDC to you
    ↓
Position updated (not removed)
```

**Frontend Display:**

Your position after partial withdrawal:
```
┌───────────────────────────────────────────┐
│  USDC Pool              $500.00  +$0.00   │
│  ─────────────────────────────────────────│
│  LP Tokens:           500.00 ↓            │
│  Pool Share:          0.33% ↓             │
│  Initial Investment:  $500.00 ↓           │
│  Current APY:         12.0%               │
│  Provided Date:       Jan 27, 2025        │
│                                           │
│  [Withdraw Liquidity]                     │
└───────────────────────────────────────────┘
```

**HashPack:**
```
Tokens:
  USDC: 5,525.00 ↑ (was 5,000, now +525)
  LP-USDC: 500.00 ↓ (was 1,000, now 500)
```

---

## Scenario 5: Multiple Users & Pool Dynamics

### Example: 3 Users Providing Liquidity

**User A (You):**
- Provides: 1,000 USDC
- Receives: 1,000 LP-USDC
- Pool share: 0.67%

**User B:**
- Provides: 5,000 USDC
- Receives: 5,000 LP-USDC
- Pool share: 3.33%

**User C:**
- Provides: 10,000 USDC
- Receives: 10,000 LP-USDC
- Pool share: 6.67%

**Pool Stats Update:**
```
┌─────────────────────────────────────┐
│  USDC Pool                          │
│  Total Liquidity: $166,000 ↑        │
│  Available: $111,000 ↑              │
│  Borrowed: $55,000                  │
│  Utilization: 33% ↓                 │
│  Current APY: 10.5% ↓               │
│                                     │
│  Total Providers: 13                │
│  Your Share: 0.60%                  │
└─────────────────────────────────────┘
```

**What This Means:**
- More liquidity = Lower utilization
- Lower utilization = Lower APY
- But more stable and liquid pool

---

## Scenario 6: High Utilization (Lots of Borrowing)

### What Happens:
```
Many farmers borrow:
    Total Borrowed: $55,000 → $140,000
    Available: $95,000 → $10,000
    Utilization: 37% → 93%
    ↓
APY increases dramatically:
    Your APY: 12% → 25%
```

**Frontend Display:**
```
┌─────────────────────────────────────┐
│  ⚠️ High Utilization Alert          │
│                                     │
│  USDC Pool                          │
│  Total Liquidity: $150,000          │
│  Available: $10,000 ⚠️              │
│  Borrowed: $140,000                 │
│  Utilization: 93% 🔴                │
│  Current APY: 25% 🚀                │
│                                     │
│  ⚠️ Limited liquidity available     │
│  Withdrawals may be delayed         │
└─────────────────────────────────────┘
```

**Your Position:**
```
┌───────────────────────────────────────────┐
│  USDC Pool              $1,000.00  +$0.00 │
│  ─────────────────────────────────────────│
│  LP Tokens:           1,000.00            │
│  Pool Share:          0.67%               │
│  Initial Investment:  $1,000.00           │
│  Current APY:         25% 🚀              │
│  Provided Date:       Jan 27, 2025        │
│                                           │
│  ⚠️ High utilization - withdrawal may     │
│     require waiting for loan repayments   │
│                                           │
│  [Withdraw Liquidity]                     │
└───────────────────────────────────────────┘
```

**If You Try to Withdraw:**
```
┌─────────────────────────────────────────────────┐
│  ⚠️ Insufficient Liquidity                      │
│                                                 │
│  You requested: 1,000 USDC                      │
│  Available in pool: 10 USDC                     │
│                                                 │
│  Please try:                                    │
│  • Withdraw smaller amount (up to 10 USDC)      │
│  • Wait for borrowers to repay loans            │
│  • Check back later                             │
│                                                 │
│  [OK]                                           │
└─────────────────────────────────────────────────┘
```

---

## Complete Frontend Flow Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                    LENDING SECTION                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  Available Pools                                    │   │
│  │                                                     │   │
│  │  • USDC Pool (8.5% APY)                            │   │
│  │  • KES Pool (12% APY)                              │   │
│  │                                                     │   │
│  │  [Provide Liquidity]                               │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  My Positions                                       │   │
│  │                                                     │   │
│  │  ┌─────────────────────────────────────────────┐   │   │
│  │  │  USDC Pool         $1,050.00  +$50.00      │   │   │
│  │  │  LP Tokens: 1,000                          │   │   │
│  │  │  APY: 12%                                  │   │   │
│  │  │  [Withdraw]                                │   │   │
│  │  └─────────────────────────────────────────────┘   │   │
│  │                                                     │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  Pool Statistics                                    │   │
│  │                                                     │   │
│  │  Total Liquidity: $150,000                         │   │
│  │  Total Borrowed: $55,000                           │   │
│  │  Utilization: 37%                                  │   │
│  │  Your Share: 0.67%                                 │   │
│  │                                                     │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  Transaction History                                │   │
│  │                                                     │   │
│  │  • Jan 27: Provided 1,000 USDC                     │   │
│  │  • Jan 28: Earned 2.50 USDC interest               │   │
│  │  • Jan 29: Withdrew 500 USDC                       │   │
│  │                                                     │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## Summary: What's Real vs Mock

### ✅ Real (Actually Happens)
1. LP token association in HashPack
2. LP token minting on Hedera
3. LP token transfer to your wallet
4. LP tokens visible in HashPack
5. LP token burning on withdrawal
6. USDC transfer back to you
7. All transactions on blockchain
8. Viewable on HashScan
9. Database records

### ⚠️ Mock (Simulated)
1. User USDC deposit (no actual transfer from you)
2. Interest calculation (hardcoded 5%)
3. APY changes (not based on real utilization)
4. Borrowing (no real loans)
5. Pool statistics (hardcoded values)
6. Utilization rate (not real)

### 🔮 Future (Not Implemented)
1. Real borrowing with collateral
2. Time-based interest accrual
3. Dynamic APY based on utilization
4. Liquidations
5. Flash loans
6. Governance

---

## Key Takeaways

1. **You get real LP tokens** in your HashPack wallet
2. **You can burn them** to get USDC back (with mock interest)
3. **All transactions** are on Hedera blockchain
4. **Borrowing is mock** - no real loans yet
5. **Interest is mock** - flat 5%, not time-based
6. **It works** for testing and demonstration
7. **Not production-ready** - needs real borrowing implementation

This is a **functional prototype** with real blockchain integration for the liquidity provider side!
