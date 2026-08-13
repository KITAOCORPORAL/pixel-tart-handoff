# Pixel Tart Physical Pointer Diagnostic DevValidation

## 结论

- Build Type：PhysicalPointerDiagnostic DevValidation
- Product HEAD：`16be311573e7b9a91b9cee2ee27484a2196acc81`
- ProductVersion：2.3.0
- FileVersion：2.3.0.0
- SchemaVersion：5
- Debug：2009/2009
- Release：2009/2009
- UserVerified：false
- PhysicalPointerVerified：false

## Single Close Authority

自动化合同确认同一个 Surface 同时只保留一个可见 X：

- 教程：1 个本地 X；Shell X 隐藏；另保留“退出教程”文字按钮。
- 拼图：1 个 Shell X；模块本地 X 已移除。
- RAW 转 JPG：1 个 Shell X；模块 Header X 已隐藏。
- 批量压缩：1 个 Shell X；模块 Header X 已隐藏。
- 整理图片、归片工作区、在线选片主界面、摄影收支主界面：只使用 Shell X。
- 模态、抽屉和详情：只使用自身 Header X；Shell X 隐藏。

自动测试中的重复关闭按钮数量为 0；这不是用户物理鼠标验收结论。

## Physical Pointer Diagnostic

诊断版按四层记录关闭点击：

1. Win32：WM_LBUTTONDOWN / WM_LBUTTONUP。
2. WPF：PreviewMouseDown / PreviewMouseUp。
3. Target：InputHitTest、VisualHitTest、祖先链、HitTest/Enabled 状态。
4. Action：Close Click、Shell Escape、Surface Closed、Tutorial Overlay Detached。

只有同一次完整物理鼠标序列和同一控件目标链才能标记物理点击。UI Automation Invoke 不会被计入 PhysicalPointerVerified。

## 安装包

- Path：`artifacts/releases/2.3.0/installer/PixelTart_2.3.0_PhysicalPointerDiagnostic_DevValidation_x64.exe`
- Size：50726626 bytes
- SHA-256：`EDA6A4672ACF300EDF98DE0AF8EC7F7219B7B7B4FD84F612446CF97F5F8FECF1`
- Provider：None
- Acceptance 数据根：必须由验收启动器显式提供；缺失时拒绝启动。
- 安装器：独立 AppId、独立目录、无自动启动、无快捷方式、不关闭正式版、不删除正式数据。

## 当前边界

本轮没有用 UIA Invoke 或自动截图冒充物理鼠标。新包尚未由用户前台真实鼠标点击，因此以下字段全部保持 false：

- tutorial_x_physical_verified
- tutorial_exit_physical_verified
- collage_x_physical_verified
- raw_x_physical_verified
- batch_x_physical_verified
- physical_pointer_verified
- installed_ui_verified
- user_verified

真实点击后应只读本机最新 `InputDiagnostics/physical-pointer-session.json`，向 Handoff 写入脱敏结论，不上传原始诊断文件。
