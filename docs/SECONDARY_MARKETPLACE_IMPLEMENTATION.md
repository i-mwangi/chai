# Secondary Marketplace Implementation Guide

## Overview
This document outlines the implementation of a peer-to-peer secondary marketplace where investors can list and trade grove tokens with each other.

## Status: ⚠️ PARTIALLY IMPLEMENTED

### ✅ Completed:
- Database schema created
- Schema definitions added
- Migration file ready

### 🔄 In Progress:
- Backend API endpoints
- Escrow system
- Frontend integration

### ❌ Not Started:
- Testing
- Error handling refinement
- UI polish

## Architecture

### Flow Diagram:
```
Investor A (Seller)                Platform                 Investor B (Buyer)
      |                               |                            |
      |--1. Create Listing----------->|                            |
      |   (price, amount)             |                            |
      |                               |                            |
      |<--2. Transfer tokens----------|                            |
      |   to escrow                   |                            |
      |                               |                            |
      |                               |<--3. Browse listings-------|
      |                               |                            |
      |                               |<--4. Buy tokens------------|
      |                               |   (pay USDC)               |
      |                               |                            |
      |<--5. Receive USDC-------------|                            |
      |                               |                            |
      |                               |--6. Transfer tokens------->|
      |                               |   from escrow              |
```

## Database Schema

### marketplace_listings
Stores active and historical listings.

```sql
- id: Unique listing ID
- seller_address: Who is selling
- grove_id: Which grove
- token_address: Token contract address
- token_amount: How many tokens
- price_per_token: Price in USDC cents
- total_price: Total cost
- status: active | sold | cancelled | expired
- created_at, expires_at, sold_at
- buyer_address: Who bought (if sold)
- transaction_hash: Blockchain proof
```

### marketplace_trades
Records completed trades for history/analytics.

### marketplace_escrow
Tracks tokens held in escrow during listing.

## API Endpoints

### 1. GET /api/marketplace/listings
Get all active listings.

**Response:**
```json
{
  "success": true,
  "listings": [
    {
      "id": "listing_123",
      "groveName": "Sunrise Valley",
      "sellerAddress": "0.0.123456",
      "tokenAmount": 100,
      "pricePerToken": 2500, // $25.00
      "totalPrice": 250000, // $2,500.00
      "createdAt": 1234567890
    }
  ]
}
```

### 2. POST /api/marketplace/create-listing
Create a new listing.

**Request:**
```json
{
  "groveId": 1,
  "tokenAmount": 100,
  "pricePerToken": 2500
}
```

**Process:**
1. Validate seller owns tokens
2. Transfer tokens to platform escrow
3. Create listing in database
4. Return listing ID

### 3. POST /api/marketplace/buy
Purchase tokens from a listing.

**Request:**
```json
{
  "listingId": "listing_123",
  "buyerAddress": "0.0.789012"
}
```

**Process:**
1. Validate listing is active
2. Transfer USDC from buyer to seller
3. Transfer tokens from escrow to buyer
4. Mark listing as sold
5. Record trade

### 4. POST /api/marketplace/cancel-listing
Cancel an active listing.

**Request:**
```json
{
  "listingId": "listing_123"
}
```

**Process:**
1. Validate seller owns listing
2. Return tokens from escrow to seller
3. Mark listing as cancelled

### 5. GET /api/marketplace/my-listings/:address
Get user's active listings.

### 6. GET /api/marketplace/trade-history/:address
Get user's trade history (bought and sold).

## Implementation Steps

### Phase 1: Database Setup ✅
1. Run migration: `node scripts/run-migrations.mjs`
2. Verify tables created
3. Test with sample data

### Phase 2: Backend API (Next)
1. Implement listing creation
2. Implement escrow system
3. Implement purchase flow
4. Implement cancellation
5. Add error handling

### Phase 3: Frontend Integration
1. Add "List Tokens" button to investor portfolio
2. Create listing modal
3. Update marketplace to show real listings
4. Add purchase flow
5. Add my listings view

### Phase 4: Testing
1. Test listing creation
2. Test token escrow
3. Test purchases
4. Test cancellations
5. Test edge cases

## Security Considerations

### Token Escrow:
- Tokens MUST be transferred to platform before listing is active
- Platform holds tokens in treasury during listing
- Tokens released only on: successful sale OR cancellation

### Payment Flow:
- Buyer pays USDC directly to seller (not through platform)
- Platform facilitates token transfer only after payment confirmed
- All transactions recorded on blockchain

### Validation:
- Verify seller owns tokens before listing
- Verify buyer has enough USDC before purchase
- Verify listing is still active before purchase
- Prevent double-spending

## User Experience

### For Sellers:
1. Go to "My Portfolio"
2. Click "List for Sale" on any grove holding
3. Enter price per token
4. Confirm token transfer to escrow
5. Listing appears in marketplace
6. Receive USDC when sold
7. Can cancel anytime (tokens returned)

### For Buyers:
1. Go to "Marketplace"
2. Browse available listings
3. Click "Buy" on desired listing
4. Confirm USDC payment
5. Receive tokens immediately
6. Tokens appear in portfolio

## Next Steps

To complete this implementation:

1. **Run the migration:**
   ```bash
   node scripts/run-migrations.mjs
   ```

2. **Implement backend API** (see api/marketplace-service.ts)

3. **Update frontend** (see frontend/js/marketplace.js)

4. **Test thoroughly**

5. **Deploy to production**

## Questions?

This is a complex feature. Key decisions needed:

1. **Escrow vs Direct Transfer**: Should platform hold tokens or facilitate direct peer-to-peer?
   - Current: Platform escrow (safer, more control)
   - Alternative: Direct transfer (more decentralized)

2. **Payment Method**: How does buyer pay seller?
   - Current: USDC transfer on Hedera
   - Alternative: Platform holds USDC too

3. **Fees**: Should platform charge trading fees?
   - Current: No fees
   - Alternative: Small percentage fee

4. **Expiration**: Should listings auto-expire?
   - Current: Optional expiration
   - Alternative: All listings expire after X days

Let me know your preferences and I'll continue the implementation!
