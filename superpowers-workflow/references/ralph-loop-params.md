# Ralph Loop 使用参考

## 命令格式

```
/ralph-loop "<prompt>" --max-iterations <N> --completion-promise "<text>"
```

## 参数动态设置

| 复杂度 | 路径 | --max-iterations | --completion-promise |
|--------|------|-----------------|---------------------|
| 低 | B 类 | 3 | 可选 |
| 中 | B/C 类 | 5 | 建议设置 |
| 高 | C 类 | 8 | 必须设置 |

## completion-promise 编写指南

- 精确描述"完成"状态，不模糊
- 可客观验证，非主观感受
- 示例:
  - 好: "所有测试通过且 code review 无 Critical issue"
  - 好: "PRD 覆盖所有功能模块且有验收标准"
  - 差: "代码看起来不错"
  - 差: "基本完成了"

## 退出条件

Ralph Loop 在以下条件任一满足时退出：
1. completion-promise 文本被 `<promise>` 标签包裹输出
2. 迭代次数达到 max-iterations
3. 人工通过 `/cancel-ralph` 取消

## 注意事项

- completion-promise 只在陈述完全真实时才能输出
- 禁止为退出循环而输出虚假 promise
- Ralph 每轮看到上一轮的文件/git 历史，可据此迭代改进
- 迭代中避免重复输出已知信息
