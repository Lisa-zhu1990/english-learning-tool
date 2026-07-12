# CHANGELOG V2.3.2

## 问题原因

- V2.3.1 中 `wrongWords` 同时承担“历史错误记录”和“当前需要复习错词”两种含义。
- 因此某个词曾经错过，即使之后复习正确，也可能继续出现在错词复习列表里。
- 本版将“历史错误次数”和“当前待复习状态”分离。

## 修改内容

- 错词复习列表只显示当前需要复习的词。
- 复习正确后，`correctStreak +1`，该词不再出现在当前错词复习列表中。
- 复习错误后，`count +1`，`lastWrong` 更新，`correctStreak` 清零，`mastered=false`。
- 连续复习正确 3 次后，自动设置 `mastered=true`。
- 点击“错词复习”时，如果没有待复习错词，提示“当前没有需要复习的错词。”。
- 学习档案里的“错词数量”改为当前待复习错词数量。

## 数据结构变化

`wrongWords` 增加 `correctStreak`：

```js
wrongWords: {
  tractor: {
    count: 5,
    lastWrong: "2026-07-12T00:00:00.000Z",
    mastered: false,
    correctStreak: 0
  }
}
```

- `count`：历史错误次数，保留不清零。
- `lastWrong`：最近一次错误时间。
- `mastered`：是否已掌握。
- `correctStreak`：连续复习正确次数。

旧数据会自动补齐 `correctStreak: 0`。

## 测试结果

- 场景1：首次听写错 2 个，错词复习显示 2 个，通过。
- 场景2：错词复习 2 个全部正确后，不再显示，通过。
- 场景3：历史错误次数保留，通过。
- 场景4：再次错误后 `count` 增加、`correctStreak` 清零，通过。
- 场景5：刷新页面后 LocalStorage 数据仍存在，通过。
- 场景6：没有待复习错词时提示“当前没有需要复习的错词。”，通过。
- 自然拼读自检：`PHONICS_TESTS` 通过，结果为 `36 passed, 0 failed`。
- 听写整词朗读：正常。

## 兼容说明

- 未修改自然拼读模块。
- 未修改 `LIBRARY`。
- 未修改教材系统。
- 未修改听写核心流程。
- 未修改批改界面布局。
- 继续使用 `english-learning-profile`，不新增重复存储。
