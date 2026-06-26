# EAMarketX.com

[eamarketx.com](https://eamarketx.com)

EAMarketX 是一个面向中文用户的 EA 交易市场，聚焦自动化交易相关数字商品的发布、购买、下载、评论与结算。
这里既展示商品，也展示平台如何把订单、文件权限、平台手续费和卖家入账拆开处理，让交易流程更清晰。

## 核心能力

- 支持 EA、MQL、SET、指标、脚本和交易工具
- 支持封面、截图、版本号、分类、标签和简介
- 支持购买后永久下载已购版本
- 支持收藏、评价、投诉和作者主页
- 支持平台托管结算、卖家余额、提现申请和自动打款

## 为什么这样设计

我们希望它看起来像一个正式、清楚、可验证的产品，而不是临时拼出来的资源站。
所以商品、订单、文件、余额和提现会被拆成不同层来处理：

- 商品层负责展示和发布
- 订单层负责确认支付和发放权限
- 文件层负责私有存储和短时效下载
- 余额层负责累计收入、冻结收入和可提现金额
- 风控层负责申诉、退款和异常处理

## 安全原理

平台采用“先托管、再结算”的交易思路，而不是买家直接把钱打给卖家。
这样做的好处很直接：

- 平台可以自动确认订单和下载权限
- 平台可以统一收取 5% 手续费
- 平台可以保留退款、投诉和风控处理能力
- 卖家余额和可提现金额可以分开记录
- 买家即使在商品下架后，仍然可以下载已购买版本

```ts
const platformFeeRate = 0.05;
const sellerPayoutRate = 0.95;
```

每一笔成功交易都会按固定比例拆分：

- 平台收入 5%
- 卖家入账 95%

## 订单与文件权限

订单不是前端自己判断是否成功，而是由后端与支付监听共同确认。
购买完成后，平台会先校验订单，再发放下载权限，最后把文件以短时效签名链接的方式提供给用户。

```ts
export async function verifyPaidOrder({ orderId, buyerId }) {
  const { data: order } = await supabase
    .from("orders")
    .select("id,status,buyer_id,total_cents,paid_at")
    .eq("id", orderId)
    .single();

  if (!order || order.buyer_id !== buyerId) return { ok: false };
  if (order.status !== "paid") return { ok: false };

  return { ok: true };
}
```

平台文件默认保存在私有存储里，只有购买者可以下载。

```sql
create policy "read only purchased files"
on storage.objects
for select
using (
  bucket_id = 'private-products'
  and exists (
    select 1
    from purchases
    where purchases.product_id = storage.objects.product_id
      and purchases.buyer_id = auth.uid()
  )
);
```

## 卖家收入与出金

卖家的收入被拆成三部分：

- 累计收入
- 冻结收入
- 可提现余额

这样可以支持 24 小时申诉窗口、人工审核和自动出金。
卖家看到的是累计收入，不会因为一次提现就把历史收入抹掉。

```ts
export async function settleSellerBalance(sellerId: string, amountCents: number) {
  const available = Math.max(0, amountCents);
  if (available <= 0) return;

  await supabase.from("seller_balances").upsert({
    seller_id: sellerId,
    available_cents: available,
    updated_at: new Date().toISOString(),
  });
}
```

## 自动化流程

平台把大量重复动作自动化了：

- 订单检测
- 支付确认
- 下载授权
- 评论资格判断
- 余额统计
- 提现状态跟踪

这让平台即使商品和用户数量继续增长，也能保持比较稳定的处理方式。

## 交易流程

1. 卖家发布商品
2. 买家下单付款
3. 平台确认到账
4. 平台发放下载权限
5. 平台记录 5% 手续费和 95% 卖家入账
6. 进入申诉期
7. 申诉期结束后，卖家余额进入可提现状态
8. 达到条件后自动出金或手动审核出金

## 技术栈

- Next.js
- Supabase
- Render
- USDT BEP20

## 公开说明

- 平台不公开用户私有文件的长期直链
- 平台不把购买权限交给前端自行决定
- 平台不把手续费和卖家收入混成一个数字
- 平台不依赖单次页面状态来判断交易最终结果

## 样例片段

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract EAMarketVault {
    address public owner;

    constructor() {
        owner = msg.sender;
    }
}
```

```ts
const payoutCode = `export async function settleSellerBalance(sellerId: string, amountCents: number) {
  const available = Math.max(0, amountCents);
  if (available <= 0) return;

  await supabase.from("seller_balances").upsert({
    seller_id: sellerId,
    available_cents: available,
    updated_at: new Date().toISOString(),
  });
}`;
```

## 站点地址

[eamarketx.com](https://eamarketx.com)

## 开源说明

公开仓库用于展示产品结构、安全思路和关键流程，方便用户理解平台是怎样完成托管、下载和结算的。
