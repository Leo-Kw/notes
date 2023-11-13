# Rollup 整体构建阶段

在执行 `rollup` 命令之后，在 cli 内部的主要逻辑简化如下:

```javascript
// Build 阶段
const bundle = await rollup.rollup(inputOptions);

// Output 阶段
await Promise.all(outputOptions.map(bundle.write));

// 构建结束
await bundle.close();
```
