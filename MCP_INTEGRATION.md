# MCP 集成说明

## MCP Server

- **地址**：https://mcp.mcd.cn
- **协议**：Streamable HTTP
- **认证**：Authorization: Bearer {MCP_TOKEN}
- **MCP 版本**：2025-06-18
- **限流**：600 次/分钟

## 调用流程

```
用户一句话
    ↓
now-time-info          → 获取当前时间，判断时段（早/午/晚/夜宵）
    ↓
query-nearby-stores    → 根据用户地址查最近门店
    ↓
auto-bind-coupons      → 一键领取麦麦省所有可用券
    ↓
query-meals            → 拉该门店可售菜单（含价格、促销标签）
    ↓
query-store-coupons    → 查该门店可用券列表
    ↓
list-nutrition-foods   → 拉全量营养数据（热量、蛋白质、脂肪等）
    ↓
【本地打分】从候选集生成 4 套方案：
  · 最省钱：券后价最低，必含主食
  · 最管饱：热量 ≥ 400kcal，蛋白质越高越靠前
  · 最低卡：热量 ≤ 用户上限，按热量升序，含饮料
  · 扛饿顶配：蛋白质最高
    ↓
用户选一套
    ↓
calculate-price        → 带 couponId 试算
    ↓
create-order           → 创建订单，返回 payH5Url
    ↓
用户支付 → query-order 查状态
```

## 使用的工具清单

| 工具 | 用途 |
|---|---|
| now-time-info | 获取当前时间，判断餐段 |
| query-nearby-stores | 定位最近门店 |
| auto-bind-coupons | 一键领券 |
| query-meals | 拉可售菜单 |
| query-store-coupons | 查门店可用券 |
| list-nutrition-foods | 拉营养数据 |
| calculate-price | 价格计算 |
| create-order | 创建订单 |
| query-order | 查订单状态 |

## 业务价值

- **对用户**：从"翻 121 道菜 + 3 张券"压缩到"一句话 + 4 套方案"
- **对麦当劳**：提高券核销率（自动领券、自动提醒过期）、提升下单转化率
- **差异化**：不是裸调点餐接口，而是把"选品 + 选券 + 凑单 + 营养 + 下单"串成完整决策链
