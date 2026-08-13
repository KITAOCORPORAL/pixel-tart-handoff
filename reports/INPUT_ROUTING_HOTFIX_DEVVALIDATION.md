# Pixel Tart Input Routing Hotfix DevValidation

## 结果

- Product commit: `c38a8015cabbf67645c6a8bbb0282b4f2f995d11`
- Build type: `InputRoutingHotfix_DevValidation`
- ProductVersion: `2.3.0`
- FileVersion: `2.3.0.0`
- SchemaVersion: `5`
- Debug: `2000/2000`
- Release: `2000/2000`
- UserVerified: `false`

## 根因与修复

旧关闭入口位于教程或模块自身的 Visual Tree 中，且教程退出会等待可能长期运行的后台步骤，导致输入或命令即使进入也无法保证立即释放界面。修复后，RootGrid 最上层提供独立 `ShellEmergencyCloseButton`，与 TutorialOverlay 为兄弟层，ZIndex 为 30000；教程 X、退出教程、模块 X 与 Window Escape 都进入同一 Shell 逃生服务。教程先同步切换 Inactive、移除 Overlay、恢复输入与工作台，再限时清理后台任务。

`WorkbenchRoot` 与 `SidebarRoot` 原先设置在普通 Grid/Border 上，这些布局元素默认没有 UI Automation peer，曾造成自动验收无法确认导航结果。现在使用不捕获鼠标的 Pane landmark 暴露稳定 AutomationId。

## Input Routing 事实

- BlockingElement: `None`
- Tutorial X: InvokePattern 可用；事件链到达 `CloseClick → SurfaceCloseRequested → ForceExitTutorialEntered`
- Tutorial Exit: InvokePattern 可用；执行后教程控件消失，WorkbenchRoot 与 SidebarRoot 恢复
- Shell X: InvokePattern 可用；事件链到达 `ForceCloseCurrentSurfaceEntered`
- RAW X / Escape: 均通过；关闭后 WorkbenchRoot 可见、启用、未离屏
- Batch X / Escape: 均通过；关闭后 WorkbenchRoot 与 SidebarRoot 可见、启用、未离屏

## 安全边界

- Computer Use 未使用；浏览器未操作。
- UIA 工具只接受 `.Acceptance.exe`，只按 AutomationId 定位目标进程。
- Acceptance EXE 缺少绝对 `PIXEL_TART_ACCEPTANCE_ROOT` 时直接拒绝启动。
- InputRouting 安装器不提供安装后自动运行，不关闭同标题正式应用，不创建删除正式用户数据的选项。
- 教程 Match/Copy 不写正式项目或匹配决策，教程任务不关联正式 ProjectId。
- 未上传原始诊断日志、数据库、绝对路径、图片、RAW、客户资料或凭据。

## 产物

- Installer: `PixelTart_2.3.0_InputRoutingHotfix_DevValidation_x64.exe`
- Size: `50723306` bytes
- SHA-256: `0B38DD0F65268FD589E5A637E1293EC3BE3E8EDADAA2B33DF5D0C59303AC966C`
- Publish payload: 280 files; no tests, PDB, TRX, database, log, image or RAW payload
- Provider: `None`

## 验证边界

Windows 前台安全策略阻止了自动工具的物理鼠标 SendInput，因此物理鼠标字段保持 `false`。本轮通过的是实际安装版的 Windows UI Automation InvokePattern 和定向 Escape。`InstalledUiVerified=true` 表示安装版自动化已验证，不代表用户验收；`UserVerified=false` 必须保持到用户亲自确认。
