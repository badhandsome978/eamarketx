# EAMarketX

[eamarketx.com](https://eamarketx.com)

EAMarketX 是一个面向中文用户的 EA 交易市场，专注于自动化交易相关数字商品的发布、购买、下载和结算。

## 为什么是 EAMarketX

我们希望它看起来像一个正式、清晰、可信的产品，而不是临时拼起来的资源站。

- 支持 EA、MQL 文件、SET 参数、指标、脚本和交易工具
- 支持封面、截图、版本号、分类、价格和标签
- 支持购买后长期下载
- 支持收藏、评价和作者主页
- 支持平台托管结算与卖家提现管理

## 安全与风控

平台采用平台托管的交易方式，而不是买家直接打给卖家。
这样可以保留手续费、售后、订单记录和争议处理能力。

```ts
const platformFeeRate = 0.05;
const sellerPayoutRate = 0.95;
```

这意味着每笔成功交易都会按固定比例分账：

- 平台收取 5%
- 卖家结算 95%

平台侧会记录：

- 订单
- 购买权限
- 收藏
- 评价
- 提现申请
- 到账状态

## 站点地址

[eamarketx.com](https://eamarketx.com)

## 技术栈

- Next.js
- Supabase
- Render
- USDT BEP20

## 公开仓库

https://github.com/badhandsome978/eamarketx

