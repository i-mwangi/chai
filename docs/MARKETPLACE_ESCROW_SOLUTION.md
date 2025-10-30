# Marketplace Escrow Solution

## Problem
When purchasing tokens from the secondary marketplace, the system was failing with `INSUFFICIENT_TOKEN_BALANCE` error because:

1. **Virtual Escrow**: When sellers list tokens, the system only updates the database (reduces their holdings) but doesn't actually transfer tokens to platform treasury
2. **Real Transfers**: When buyers purchase, the system tries to transfer real tokens from treasury to buyer
3. **Mismatch**: Treasury doesn't have the tokens that were "virtually escrowed"

## Temporary Solution (Current Implementation)

### Listing Tokens:
- Seller's token holdings are reduced in database (virtual escrow)
- **No actual blockchain transfer** happens
- Tokens remain in seller's Hedera account

### Purchasing Tokens:
1. **Mint new tokens** to treasury (equal to purchase amount)
2. **Transfer tokens** from treasury to buyer
3. Update database holdings for buyer

### Why This Works:
- Buyer receives real tokens on Hedera blockchain
- Database accurately tracks ownership
- No dependency on seller's actual token balance
- Platform can always fulfill purchases

### Limitations:
- Increases total token supply (inflationary)
- Seller keeps their original tokens (they're not actually sold)
- Not a true peer-to-peer marketplace

## Proper Solution (Future Implementation)

### Option 1: Real Escrow with Allowances
```typescript
// When listing:
1. Seller approves platform as spender (allowance)
2. Platform transfers tokens from seller to treasury
3. Tokens held in escrow until purchase or cancellation

// When purchasing:
1. Transfer tokens from treasury to buyer
2. Transfer USDC from buyer to seller
3. Update database
```

### Option 2: Atomic Swap
```typescript
// When purchasing:
1. Create multi-party transaction
2. Seller transfers tokens to buyer
3. Buyer transfers USDC to seller
4. Both happen atomically or neither happens
```

### Option 3: Smart Contract Escrow
```solidity
// Deploy escrow contract
1. Seller deposits tokens to contract
2. Buyer deposits USDC to contract
3. Contract executes swap automatically
4. Handles cancellations and refunds
```

## Implementation Steps for Real Escrow

### 1. Add Escrow Transfer on Listing
```typescript
export async function listTokensForSale(req, res) {
    // ... validation ...
    
    // Transfer tokens to platform treasury (escrow)
    const escrowResult = await hederaTokenService.transferTokensBetweenAccounts(
        tokenAddress,
        sellerAddress,  // from
        platformTreasuryId,  // to (escrow)
        tokenAmount,
        `Escrow for marketplace listing`
    );
    
    if (!escrowResult.success) {
        return sendError(res, 500, 'Failed to escrow tokens');
    }
    
    // ... create listing ...
}
```

### 2. Transfer from Escrow on Purchase
```typescript
export async function purchaseFromMarketplace(req, res) {
    // ... validation ...
    
    // Transfer from escrow to buyer
    const transferResult = await hederaTokenService.transferTokens(
        tokenAddress,
        buyerAddress,
        tokenAmount,
        `Marketplace purchase`
    );
    
    // ... update database ...
}
```

### 3. Return Escrow on Cancellation
```typescript
export async function cancelListing(req, res) {
    // ... validation ...
    
    // Return tokens from escrow to seller
    const returnResult = await hederaTokenService.transferTokens(
        tokenAddress,
        sellerAddress,
        tokenAmount,
        `Marketplace listing cancelled`
    );
    
    // ... update database ...
}
```

## Current Workaround Usage

The current implementation works for testing and development:

✅ **Pros:**
- Buyers get real tokens
- No complex escrow logic needed
- Works without seller signatures
- Simple to implement

❌ **Cons:**
- Inflates token supply
- Seller keeps original tokens
- Not economically accurate
- Can't be used in production

## Migration Path

1. **Phase 1** (Current): Mint-and-transfer for testing
2. **Phase 2**: Implement allowance-based escrow
3. **Phase 3**: Add atomic swaps for better UX
4. **Phase 4**: Deploy smart contract escrow for full decentralization

## Testing the Current Solution

```javascript
// List tokens (virtual escrow)
await api.post('/api/marketplace/list-tokens', {
    groveId: 1,
    tokenAmount: 10,
    pricePerToken: 30,
    durationDays: 30,
    sellerAddress: '0.0.123456'
});

// Purchase tokens (mint + transfer)
await api.post('/api/marketplace/purchase', {
    listingId: 1,
    tokenAmount: 5,
    buyerAddress: '0.0.789012'
});

// Buyer receives 5 real tokens
// Seller still has their original tokens
// Total supply increased by 5
```

## Notes

- This is a **temporary development solution**
- Do not use in production with real value
- Implement proper escrow before mainnet launch
- Consider regulatory implications of token minting
