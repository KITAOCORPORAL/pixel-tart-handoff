# CoreReliability Interaction Hotfix

## 这次修复了什么

这次只修复弹窗、覆盖层和教程退出的可靠性，不增加新的业务功能，也不进入 RC3。

以前的问题是：不同页面各自处理关闭和取消，教程退出还会异步请求关闭主窗口；后台操作没有统一等待合同，Esc 在部分页面没有对应处理。教程第 18 步还只显示“导出失败”，没有告诉用户哪个报告文件缺失。

现在所有关闭/取消请求都经过 `ModalHost` 合同。取消会等待后台任务结束，失败后仍可以再次取消或退出。教程退出会取消并等待当前动作、释放覆盖层、恢复普通工作区，并保留用户项目数据。第 18 步会实际检查三个文件：匹配报告 CSV、匹配报告 JSON、操作日志 TXT；失败时列出缺失文件，重试会重新执行导出。

## 门禁结果

- Debug：1974/1974，通过，0 失败、0 跳过、0 警告、0 错误。
- Release：1974/1974，通过，0 失败、0 跳过、0 警告、0 错误。
- DPI：Debug 101/101，Release 101/101。
- 安装包：`PixelTart_2.3.0_CoreReliability_InteractionHotfix_DevValidation_x64.exe`。
- 安装包 SHA-256：`2E305D62403AB7D0BE996117E5335198694DD5CF9905112641ACCA247A8F54C4`。
- 产品版本 2.3.0，文件版本 2.3.0.0，SchemaVersion 5，Provider=None。

## 安装版边界

安装包已安装到独立验证目录并启动。教程退出按钮实际点击后覆盖层消失、工作台可用；未把这一次结果扩大为整套安装版验收。弹窗取消、Step18 失败恢复、Esc 全矩阵和四条 Golden Path 仍由用户在前台完成，因此 `InstalledUiVerified=false`、`UserVerified=false`。

## 安全

验证只使用独立临时目录和测试副本。没有读取或写入真实 LocalAppData、生产数据库、客户资料或真实输出；没有上传 RAW、日志、凭证、密钥或路径。handoff 图片均为脱敏 UI 参考画面。

## 请求

请先审查本报告和 `codex_to_gpt.md`，再按 `RC2_FOREGROUND_ACCEPTANCE.md` 与本轮交互清单进行前台验收。未完成用户确认前不要进入 RC3。
