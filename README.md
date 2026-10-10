# 1024能量站 · 麦当劳点餐参谋

> 别人做的是让 AI 帮你下单，这个做的是让 AI 帮你**决定**下什么单。

## 这是什么

一个基于麦当劳 MCP 的 WorkBuddy Skill。加班到深夜、打开麦当劳 App 面对 121 道菜和 3 张券不知道怎么选？一句话告诉它你的预算、热量或饱腹度，它自动串起门店、菜单、优惠券、营养数据，给你 4 套方案（最省 / 最饱 / 最低卡 / 扛饿顶配），每套附"≈ 慢跑 X 分钟"，选完一键下单到支付链接。

## 目标用户

加班赶版的程序员。下午三点犯困、晚上九点饿了、减脂期嘴馋——不想翻菜单，只想知道"我该点什么"。

## 怎么用

### 1. 申请 MCP Token
去 [open.mcd.cn](https://open.mcd.cn/mcp) 手机号登录 → 控制台 → 激活，Token 即刻生成。

### 2. 配置 MCP
在 WorkBuddy 的 MCP 管理里添加：
```json
{
  "mcpServers": {
    "mcd-mcp": {
      "type": "streamablehttp",
      "url": "https://mcp.mcd.cn",
      "headers": {
        "Authorization": "Bearer YOUR_MCP_TOKEN"
      }
    }
  }
}
```

### 3. 创建 Skill
在 WorkBuddy 里新建一个 Skill，把系统提示词粘进去（见下方"系统提示词"）。

### 4. 直接说话
```
加班好饿，30 块以内
最近减脂，别超 500 大卡
我在深圳南山，想吃麦当劳
```

## 实际演示效果

输入"加班好饿，30 块以内"，49 秒内返回：
- 自动定位最近门店（南宁爱琴海餐厅，距离 573m，24h 营业）
- 识别当前时段（夜宵 22:14-23:59）
- 一键领券 + 查到 3 张可用券
- 4 套方案，每套标注价格、热量、蛋白质、券减金额
- 主动提醒"前两张券今晚 23:59 过期"
- 选方案后自动下单，返回支付链接
- 发现价格不对（三件套已含中薯，再用薯券多花 9.9）主动提醒

## 系统提示词

```
你是「1024能量站」，一个麦当劳点餐参谋。

用户说话后，按这个顺序调工具：
1. 先调 now-time-info 拿到当前时间
2. 调 query-nearby-stores 查附近门店，取第一家
3. 调 query-meals 拉这家店的可售菜单
4. 调 auto-bind-coupons 一键领券
5. 调 query-store-coupons 查这家店可用的券
6. 调 list-nutrition-foods 拉营养数据

不要问用户问题，直接开始调工具。

调完后，给用户 4 套方案：最省钱、最管饱、最低卡、扛饿顶配。
每套方案卡片格式：
- 标题行：方案字母 + 一句话定位 + 总价
- 商品逐项
- 合计热量 kcal、蛋白质 g
- 最后一行：🔥 这顿 ≈ 慢跑 X 分钟（X = 总热量 ÷ 8，四舍五入）

用户选方案后，调 calculate-price 试算，再调 create-order 下单，返回支付链接。
```

## 用了哪些 MCP 工具

9 个：now-time-info、query-nearby-stores、query-meals、auto-bind-coupons、query-store-coupons、list-nutrition-foods、calculate-price、create-order、query-order

详见 [MCP_INTEGRATION.md](./MCP_INTEGRATION.md)。

## 参赛声明

详见 [CONTEST_DECLARATION.md](./CONTEST_DECLARATION.md)。
