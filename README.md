# EAMarketX

[Visit the live site](https://eamarketx.com)

EAMarketX is a Chinese-first EA marketplace for trading robots, MQL files, SET presets, indicators, scripts, and trading tools.

## What it offers

- Sellers can publish EA files with covers, screenshots, categories, versions, and pricing.
- Buyers can purchase, download, favorite, and review products.
- Purchased downloads stay available even if the original listing is later removed.
- The platform is designed around escrow-style checkout and seller payout controls.

## Safety model

```ts
// Platform fee is fixed at 5%.
const platformFeeRate = 0.05;
const sellerPayoutRate = 0.95;
```

Transactions are designed to go through the platform first so the system can support:

- automatic fee collection
- purchase records
- seller balance tracking
- withdrawal review
- refund and dispute handling

## Tech stack

- Next.js
- Supabase
- Render
- USDT BEP20 payments

## Links

- Website: https://eamarketx.com
- GitHub: https://github.com/badhandsome978/eamarketx

