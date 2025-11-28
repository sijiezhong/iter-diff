# VSCode 代码评审助手（iter-diff）

VSCode 插件，面向技术负责人、项目经理与评审者，通过「选择基准版本 → 选择目标版本」两步聚合指定成员在版本间的全部变更，显著缩短传统“找 commit → 看提交 → 翻文件”的流程。

## 核心能力

- **范围选择**：命令面板提供 `Code Review: Set Base Ref` 与 `Code Review: Set Target Ref`，Quick Pick 自动列出所有 Tag/Branch，状态栏实时显示当前选择，点击即可重新设置。
- **成员自动识别**：完成范围设置后自动扫描 Git 提交作者，按邮箱去重、按提交次数倒序，Tree View 默认全选，可多选/取消，空结果会提示“未发现提交者”。
- **变更文件树**：通过“查看变更文件”命令生成与仓库结构一致的树，文件节点显示 `新增行数-删除行数` 与主要修改者，支持顶部搜索过滤；`codeReviewer.maxFiles` 控制展示上限，`codeReviewer.ignoreFiles` 过滤无关文件。
- **聚合 Diff 查看**：单成员模式汇总该成员所有修改，多成员模式将多个成员的合并结果与基准版本对比，完全采用 VSCode 原生 Diff + `TextDocumentContentProvider` 虚拟文档，不落地临时文件。
- **可取消与提示**：所有 Git 操作支持 `ESC` 取消，错误会同时展示在右下角通知与输出面板，便于排查。

## 使用流程

1. 打开项目，执行 `Set Base Ref` 与 `Set Target Ref`。
2. 等待成员扫描完成，在侧边栏 Tree View 中确认需要关注的成员。
3. 点击「查看变更文件」按钮/命令生成文件树。
4. 在树中点击任意文件，即可打开聚合 Diff 进行评审。

## 配置项

```json
{
  "codeReviewer.maxFiles": 200,
  "codeReviewer.ignoreFiles": [
    "*.lock",
    "dist/**",
    "**/*.min.js"
  ]
}
```

## 性能与兼容

- VSCode 1.80+、Git 2.20+，适配 Windows / macOS / Linux。
- 目标性能：1000 次提交内成员扫描 < 1s；200 个文件的树加载 < 2s；CPU 占用 < 10%（持续 5s）。
- 保持内存稳定，无 WebView，全程使用原生组件（Quick Pick / Tree View / Diff）。

## MVP 范围声明

- **不包含**：中间 commit 列表、单次 commit Diff、评审报告导出、统计图、代码质量扫描、第三方集成、历史缓存等功能。

欢迎在 Issue 中反馈需求或建议，让我们共同提升 VSCode 中的迭代评审效率。
