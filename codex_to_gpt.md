# Codex → GPT Handoff

Protocol: pixel-tart-handoff/v1
ReportId: click-routing-fix-devvalidation-20260813
CreatedAt: 2026-08-13T18:49:29+08:00
Project: Pixel Tart

## Git
Branch: feature/pixel-tart-product-redesign
HEAD: 4dac5f8e4460b7a67309646b6133bd186c121fea
WorkingTreeClean: true

## Version
ProductVersion: 2.3.0
FileVersion: 2.3.0.0
SchemaVersion: 5

## 本轮完成
基于用户真实物理会话 `PT-INPUT-20260813-003 / pointer-009`，确认 Win32 与 WPF Preview Down/Up 均到达 `TutorialExitButton`，但旧版未生成 `Button.Click`。

高置信根因是教程布局在按钮按下后重新聚焦教程目标：按钮获得焦点会触发布局刷新，旧逻辑随后将焦点夺回当前教程目标，导致 Release 模式按钮在 MouseUp 前丢失按压/捕获状态，因此不生成 Click。旧会话未直接记录 Mouse Capture、IsPressed 或按钮实例 ID，所以该中间状态是源码机制与八次重复真实会话共同支持的高置信结论，不冒充直接诊断记录。

本轮新增仅限 Escape 类控件的 PointerDown 路径：教程文字退出、教程唯一 X、Shell/Modal/Drawer 关闭 X 在真实 PointerDown 到达后立即进入现有 Shell Escape；普通保存、删除、转换、复制和导出仍使用标准 Click。键盘与 UI Automation 的标准 Button.Click 路径保留。同一输入事件只分发一次。

物理诊断新增 Down/Up 目标、Mouse Capture、IsPressed、按钮实例和 CanExecute 快照，并修复 PointerDown 在 MouseUp/Click 前成功关闭时的会话关联；UIA、Command 和普通按钮不能冒充物理 PointerDown。

## 测试
Debug: 2017/2017 (Core 1136 + WPF 780 + DPI 101), 0 failed, 0 skipped
Release: 2017/2017 (Core 1136 + WPF 780 + DPI 101), 0 failed, 0 skipped
DPI: 101/101

专项：输入路由与物理诊断 18/18；Acceptance + InputRoutingDiagnostics 构建 0 warning / 0 error。

## Installed UI
CodeVerified: true
AutomatedVerified: true
InstalledUiVerified: false
PhysicalPointerVerified: false
TutorialExitPhysicalVerified: false
TutorialXPhysicalVerified: false
CollageXPhysicalVerified: false
UserVerified: false

新包尚未由用户安装并完成真实物理鼠标复验，任何自动测试或代码审计均未将上述字段改为 true。

## 安装包
Path: artifacts/releases/2.3.0/installer/PixelTart_2.3.0_ClickRoutingFix_DevValidation_x64.exe
SHA256: F9870E01E4B7CC763B68FB2DC1992B580D97222C7BA1488F9BE66CBCE0A042DC
BuildType: ClickRoutingFix_DevValidation
Size: 50729141 bytes

安装定义使用独立 AppId、独立安装目录、Acceptance 主程序和独立数据根；开始菜单、可选桌面快捷方式与完成页启动均指向本包 Acceptance EXE。未覆盖旧包，未触碰正式 LocalAppData。

## UI证据
本轮未上传截图、原始物理 Session、坐标、日志、照片、RAW、数据库或用户数据。公开仓库仅保留脱敏结论。

## 未验证项目
1. 安装新包后，用真实鼠标点击教程“退出教程”，必须立即退出。
2. 再次进入教程，用真实鼠标点击唯一 X，必须立即退出。
3. 打开拼图，确认右上角仅一个 X，并用真实鼠标点击后立即返回。

旧会话字段：Mouse Capture Down、Mouse Capture Up、Down/Up 同一按钮实例均未被旧版诊断记录；不能从源码推断改写为已实测。

## 请求GPT审查
请审查 Escape attached action 的限定范围、PointerDown 与 Click 的去重、教程布局不再于按下期间夺焦、物理诊断关联门槛及独立 DevValidation 安装隔离。保持 state=waiting_for_gpt_review 与 UserVerified=false，等待用户完成上述三项真实物理验收。
