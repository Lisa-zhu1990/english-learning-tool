# CHANGELOG V2.3.1

## 新增功能

- 新增“一键全部正确”按钮，方便家长快速确认整轮全对。
- 新增 Unit 掌握状态字段：
  - `mastered`
  - `masterScore`
  - `lastMastered`
- 学习档案新增“掌握状态”和“掌握次数”显示。
- 已掌握 Unit 进入练习时显示提示，但不禁止继续复习。

## 批改逻辑调整

- 批改勾选含义从“错词”改为“正确词”。
- 未勾选单词自动进入错词。
- “完成批改”后按勾选数量计算正确词数。
- 错词系统继续使用未勾选单词累计 `wrongWords`。

## Unit 掌握规则

- 每次完成批改后，如果本轮 `correctWords == totalWords`，则 `masterScore +1`。
- 当 `masterScore >= 3` 时，标记 `mastered = true`。
- 记录 `lastMastered` 为最近一次达到掌握状态的时间。
- 非 100% 正确时，本版本不降低 `masterScore`。

## 数据结构变化

Unit 档案增加：

```js
{
  mastered: false,
  masterScore: 0,
  lastMastered: ""
}
```

旧学习档案会在读取时自动补齐这些字段。

## 测试结果

- 勾选正确单词：通过。
- 未勾选自动成为错词：通过。
- `wrongWords` 统计正确：通过。
- 学习档案正确显示掌握状态和掌握次数：通过。
- Unit 掌握状态保存：通过。
- 刷新后数据仍存在：通过。
- 错词复练不受影响：通过。
- 听写不受影响：通过。
- 自然拼读不受影响，`PHONICS_TESTS` 结果为 `36 passed, 0 failed`。

## 兼容性说明

- 未修改 `LIBRARY` 结构。
- 未修改自然拼读模块。
- 未修改听写整词朗读核心流程。
- 未修改错词复习的数据存储键，仍使用 `english-learning-profile`。
