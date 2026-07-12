# CHANGELOG V2.2

## 1. 新增功能

- 新增 Learning Profile（学习档案），按教材和 Unit 记录学习情况。
- 新增当前 Unit 档案展示，放在“本轮结果”区域下方。
- 新增累计练习次数、累计练习词数、累计正确词数、平均正确率、最近练习时间。
- 新增高频错词统计，显示当前 Unit 最容易错的前三个词。
- 新增 `exportLearningProfile()` 和 `importLearningProfile()` 空函数，预留 V2.4 导入导出。

## 2. 数据结构

```js
learningProfile = {
  PU2: {
    Unit1: {
      practiceCount: 0,
      totalWords: 0,
      correctWords: 0,
      accuracy: 0,
      lastPractice: "",
      wrongWords: {}
    }
  }
}
```

## 3. 修改模块

- 人工批改完成后更新学习档案。
- 生成学习、开始听写不会写入学习档案。
- 练习来源使用批改前生成的 Unit 快照，避免切换 Unit 后记录到错误档案。
- 切换 Unit 后，学习档案展示自动切换到对应 Unit。
- 自定义词库不写入 Unit 学习档案。

## 4. LocalStorage 变化

- 新增独立存储键：`english-learning-profile`。
- 原有自定义词库存储键 `english-dictation-content` 保持不变。
- 学习档案与自定义词库分开保存，互不覆盖。

## 5. 测试结果

- 新用户第一次完成批改：自动创建 Unit 档案。
- 多次完成批改：`practiceCount` 正常累加。
- 正确率：按累计 `correctWords / totalWords` 正确计算。
- 错词：错误次数正常累计。
- 刷新页面：LocalStorage 中的学习档案仍存在并可显示。
- 切换 Unit：显示对应 Unit 的档案。
- 切换教材：数据结构兼容未来 PU3 / Oxford / RAZ。
- 听写：仍只播放完整单词。
- 批改：家长勾选错误正常。
- 错词复练：正常。
- 自然拼读：`PHONICS_TESTS` 通过，结果为 `36 passed, 0 failed`。
- TTS：听写流程未调用自然拼读拆读。
- PU2 词库：正常。

## 6. 后续扩展说明

- V2.4 可在 `exportLearningProfile()` 和 `importLearningProfile()` 中实现学习档案导入导出。
- 本版本不开发日期日历、打卡、连续签到。
- 本版本不开发 V2.4 导入导出 UI。
