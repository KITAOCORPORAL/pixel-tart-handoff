# Codex → GPT Handoff

Protocol: pixel-tart-handoff/v1
ReportId: input-routing-hotfix-devvalidation-20260813
CreatedAt: 2026-08-13T14:17:52+08:00
Project: Pixel Tart

## Git
Branch: feature/pixel-tart-product-redesign
HEAD: c38a8015cabbf67645c6a8bbb0282b4f2f995d11
WorkingTreeClean: true

## Version
ProductVersion: 2.3.0
FileVersion: 2.3.0.0
SchemaVersion: 5

## 本轮完成
完成 Input Routing / HitTest / Emergency Escape P0 Hotfix。教程 X、教程“退出教程”、Shell 顶层 X、RAW X/Escape、批量压缩 X/Escape 统一由 Shell 逃生路径处理，不依赖模块 Command、CanExecute、IsBusy 或任务状态。教程退出先立即移除界面和恢复工作台，再在 500ms 安全窗口内执行后台清理；旧教程会话不能继续写入正式项目。Acceptance 程序缺少显式隔离根时会拒绝启动，验收安装器不会自动运行、不会关闭正式应用，也不会删除正式 LocalAppData。未进入 RC3，未修改版本或 Schema。

## 测试
Debug: 2000/2000 (Core 1136 + WPF 763 + DPI 101), 0 failed, 0 skipped, 0 warnings, 0 errors
Release: 2000/2000 (Core 1136 + WPF 763 + DPI 101), 0 failed, 0 skipped, 0 warnings, 0 errors
DPI: 101/101

## Installed UI
CodeVerified: true
AutomatedVerified: true
InstalledUiVerified: true
UserVerified: false
ValidationMethod: WindowsUIAutomation
ComputerUseUsed: false
TutorialXUiAVerified: true
TutorialExitUiAVerified: true
ShellXUiAVerified: true
RawXUiAVerified: true
RawEscapeVerified: true
BatchXUiAVerified: true
BatchEscapeVerified: true
WorkbenchRestoredVerified: true
SidebarRestoredVerified: true

## 安装包
Path: artifacts/releases/2.3.0/installer/PixelTart_2.3.0_InputRoutingHotfix_DevValidation_x64.exe
SHA256: 0B38DD0F65268FD589E5A637E1293EC3BE3E8EDADAA2B33DF5D0C59303AC966C
BuildType: InputRoutingHotfix_DevValidation
Size: 50723306 bytes

## UI证据
`ui-review/input-routing/` 包含脱敏 UI Automation 控件矩阵、事件链、发布审计和验证边界。正式定位全部使用稳定 AutomationId。安装版中目标按钮均为可见、启用、未离屏且支持 InvokePattern；关闭后 WorkbenchRoot 与 SidebarRoot 均可见、启用、未离屏。诊断链明确记录 CloseClick/SurfaceCloseRequested/ForceExitTutorialEntered 或 ForceCloseCurrentSurfaceEntered。BlockingElement 为 None；事件已到达按钮。未上传截图、原始日志、绝对路径、数据库、照片或 RAW。

## 未验证项目
Windows 安全策略阻止了自动化工具取得前台鼠标控制，因此没有把物理鼠标 SendInput 标记为通过；本轮通过的是实际安装版上的 Windows UI Automation InvokePattern 与定向 Escape。UserVerified 必须继续保持 false，直到用户亲自在前台确认。上一轮 GlobalSurfaceClose 的自动结论已被本轮结果取代。

## 请求GPT审查
请审查产品提交、2000/2000 Debug/Release 结果、安装包哈希、Shell 逃生层级、教程立即退出与会话隔离、Acceptance fail-closed 和卸载安全边界。不要把 Windows UI Automation 验证推断为用户验收或物理鼠标验收。
