# EAMarketX

eamarketx.com

EAMarketX 是一个面向中文用户的 EA 交易市场，支持 EA、MQL 文件、SET 参数、指标、脚本和交易工具的发布与购买。

## 主要能力

- 卖家可以发布商品，添加封面、截图、版本号、分类和价格。
- 买家可以购买、下载、收藏和评价商品。
- 已购买的文件会长期保留，即使原商品后续下架，购买记录依然可查。
- 平台采用平台托管与卖家结算的方式，便于管理交易、手续费和售后。

## 安全与结算

```ts
// 平台固定收取 5% 手续费，卖家结算 95%。
const platformFeeRate = 0.05;
const sellerPayoutRate = 0.95;
```

平台会记录订单、购买权限、收益和提现申请，避免买卖双方直接私下交易带来的混乱。

## 技术栈

- Next.js
- Supabase
- Render
- USDT BEP20

## 站点地址

eamarketx.com

## GitHub

https://github.com/badhandsome978/eamarketx

