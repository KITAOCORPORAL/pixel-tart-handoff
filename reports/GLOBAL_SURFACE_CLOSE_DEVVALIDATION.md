# Global Surface Close DevValidation

ReportId: global-surface-close-devvalidation-20260813

## 结果

Pixel Tart 2.3.0 的 Global Module Close / Shell Escape Hatch 已完成代码和自动化门禁。产品仓库保持干净，未进入 RC3，未生成正式发布版。

## 基线

- Branch: `feature/pixel-tart-product-redesign`
- HEAD: `f46a520031ea00763ed792596eee2e1679402b09`
- ProductVersion: `2.3.0`
- FileVersion: `2.3.0.0`
- SchemaVersion: `5`
- Debug: `1991/1991`, 0 failed, 0 skipped
- Release: `1991/1991`, 0 failed, 0 skipped

## 安装包

- Path: `artifacts/releases/2.3.0/installer/PixelTart_2.3.0_GlobalSurfaceClose_DevValidation_x64.exe`
- SHA256: `1C974713ECF5BC57BCAC9663C15F2B265EA251CB1F39586A78617DF16931BAA9`
- Build type: `GlobalSurfaceClose_DevValidation`
- Provider: `None`
- DevValidation 包未签名；不代表正式发布签名。
- 既有 InteractionHotfix DevValidation 包未覆盖、未替换；本轮新包使用独立文件名。

## 安装版前台检查

在独立验证目录启动候选安装版，未读取真实项目数据库、真实客户资料或真实用户数据目录。已实际确认：

- 教程“退出教程”按钮关闭覆盖层并回到工作台。
- 教程标题栏 X 关闭覆盖层并回到工作台。
- RAW 转 JPG 标题栏 X 返回工作台。
- 批量压缩标题栏 X 返回工作台。
- RAW 页面按 Escape 返回工作台。
- 批量压缩页面按 Escape 返回工作台。

本轮未把“任务继续运行时关闭页面”标记为已验证；该项仍为待验收。`InstalledUiVerified` 和 `UserVerified` 因此保持 false。

## 证据与隐私

`ui-review/global-close/` 中的七张图片是黑底文字型脱敏参考图，配有逐张 JSON 元数据。它们不包含客户照片、头像、RAW、文件内容、完整本机路径或生产数据；第七张明确标记为待验证。没有上传真实桌面截图。

## 安全边界

验证输出使用独立临时目录。没有复制、移动、删除或覆盖真实原片；没有上传凭证、私密配置或生产数据库。用户实际确认前不得把 `UserVerified` 改为 true。

## 后续请求

请 GPT 审查本交接的提交、测试计数、安装包哈希、脱敏证据及未验证项目。完成用户前台确认后再更新相应字段；本轮到此停止，不进入 RC3。
