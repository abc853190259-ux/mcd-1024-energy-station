# WorkBuddy 开发对话记录

> 本文件保留使用 WorkBuddy 开发本 Skill 的完整对话上下文，用于 WorkBuddy 专项奖励评审。

## 开发过程

### Day 0（10.9）：方案设计
- 在 WorkBuddy 中与产品经理 Skill 讨论参赛方向
- 确定核心差异化：不是"帮你下单"，而是"帮你决定下什么单"
- 基于麦当劳 MCP 官方文档（open.mcd.cn/mcp/doc）梳理 9 个核心工具

### Day 1（10.9 晚）：Skill 创建与调试
- 通过 WorkBuddy 的 skill-creator 创建「麦当劳点餐参谋」Skill
- 粘贴系统提示词，测试"加班好饿，30 块以内"
- 第一次测试即跑通全链路：门店定位 → 菜单 → 领券 → 方案推荐
- 自动发现并修正：工具名从 sellable-meals 改为 meals、price-calculate 改为 calculate-price
- 补充卡路里换算规则：每张方案卡片末尾加"🔥 这顿 ≈ 慢跑 X 分钟"

### Day 2（10.10）：端到端验证
- 测试减脂场景："最近减脂，别超 500 大卡"
- 验证 4 套方案均带卡路里换算
- 验证下单闭环：选方案 → 价格计算 → 创建订单 → 支付链接
- AI 自动处理异常：外带报错改堂食、发现价格不对主动提醒

## 最终 Skill 提示词

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
