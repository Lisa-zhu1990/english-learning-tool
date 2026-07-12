# CHANGELOG V2.3

## 1. 新增功能

- 新增 Wrong Word Review（错词复习）入口。
- 新增当前教材范围内的错词列表展示。
- 错词列表按错误次数从高到低排序。
- 新增错词复习模式，复用现有听写与人工批改流程。
- 新增“已掌握”标记，降低该词复习优先级，但不删除历史记录。
- 学习档案新增“错词数量”显示。

## 2. 数据结构变化

`wrongWords` 从数字计数升级为对象结构：

```js
wrongWords: {
  tractor: {
    count: 5,
    lastWrong: "2026-07-12T00:00:00.000Z",
    mastered: false
  }
}
```

- `count`：累计错误次数。
- `lastWrong`：最后一次错误时间。
- `mastered`：是否已标记掌握。
- 兼容旧数据：旧的 `tractor: 5` 会按需转换为 `{ count: 5, lastWrong: "", mastered: false }`。

## 3. 修改模块

- 修改学习档案错词统计逻辑，支持 `count / lastWrong / mastered`。
- 修改错词排序逻辑：未掌握优先，错误次数高优先，已掌握排后。
- 修改错词复习批改逻辑：再次勾错会增加 `count` 并更新 `lastWrong`。
- 保持听写模式整词朗读，不调用自然拼读拆读。
- 保持 `LIBRARY`、自然拼读模块、学习档案基础结构不变。

## 4. 测试结果

- 家长勾选错误：可自动进入 `wrongWords`。
- 错误次数：可正常累计。
- 错词复习：可以进入并显示正确单词。
- 错词排序：错误次数高的词排在前面。
- 错词复习听写：播放完整单词，不调用 `buildChunks()` / `splitWord()`。
- 再次错误：`count` 增加，`lastWrong` 更新。
- 已掌握：可标记，不删除历史，排序降低优先级。
- LocalStorage：继续使用 `english-learning-profile`，刷新后数据仍存在。
- 听写：正常。
- 批改：正常。
- 学习档案：正常显示错词数量与高频错词。
- 自然拼读：`PHONICS_TESTS` 通过，结果为 `36 passed, 0 failed`。
- PU2 词库：正常。

## 5. 后续扩展说明

- 后续可基于 `count / lastWrong / mastered` 开发智能复习排序。
- 后续可增加“只复习最近错词”“只复习未掌握词”等筛选。
- 后续可在 V2.4 结合学习档案导入导出。
