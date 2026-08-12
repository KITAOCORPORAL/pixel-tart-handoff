# Codex → GPT Handoff

Protocol: pixel-tart-handoff/v1
ReportId: core-reliability-interaction-hotfix-20260812
CreatedAt: 2026-08-12T18:05:00+08:00
Project: Pixel Tart

## Git
Branch: feature/pixel-tart-product-redesign
HEAD: 51044d65dfedee52e595f648cc6167e3b934c5d8
WorkingTreeClean: true

## Version
ProductVersion: 2.3.0
FileVersion: 2.3.0.0
SchemaVersion: 5

## 本轮完成

本轮是 CoreReliability Interaction Hotfix DevValidation，产品业务范围冻结，不进入 RC3。

- 增加统一的 `IModalSession` / `IModalHost` 合同和可重试的取消、关闭流程。
- 教程退出改为可等待、幂等的安全退出，不再通过关闭主窗口退出教程。
- 教程 Esc 进入同一安全退出路径；页面弹窗 Esc 进入取消路径。
- Step18 按真实磁盘结果检查 CSV、JSON、操作日志 TXT，并记录 expected、generated、missing。
- Step18 失败可以重试，不会把失败步骤锁成不可退出状态。
- 增加 Modal、Overlay、Tutorial 的专项合同测试和根因审计报告。
- 未新增业务功能，未修改版本或 Schema，未创建 RC3、Tag 或安装正式版。

## 测试

Debug: 1974/1974 通过（Core 1126、WPF 747、DPI 101；0 失败、0 跳过、0 警告、0 错误）
Release: 1974/1974 通过（Core 1126、WPF 747、DPI 101；0 失败、0 跳过、0 警告、0 错误）
DPI: Debug 101/101；Release 101/101
专项: ModalCloseSmokeTests 17/17；ModalInteractionContractTests 4/4；Onboarding ExitAsync 通过

## Installed UI

CodeVerified: true
AutomatedVerified: true
InstalledUiVerified: false
UserVerified: false

独立 DevValidation 安装版已启动于隔离目录。实际点击“退出教程”后，教程覆盖层消失并回到工作台；这只证明教程退出路径，不等同于整套 InstalledUiVerified。其余弹窗取消、Step18 失败恢复和四条 Golden Path 仍等待用户前台确认。

## 安装包

Path: artifacts/releases/2.3.0/installer/PixelTart_2.3.0_CoreReliability_InteractionHotfix_DevValidation_x64.exe
SHA256: 2E305D62403AB7D0BE996117E5335198694DD5CF9905112641ACCA247A8F54C4
SizeBytes: 50702668
BuildType: CoreReliability_InteractionHotfix_DevValidation
Publish: self-contained win-x64；WinExe；Provider=None

## UI证据

证据目录：`ui-review/interaction-hotfix/`

- `01_tutorial_step18_failed.png`：Step18 失败状态参考画面，非安装版证据。
- `02_tutorial_exit_success.png`：教程退出后工作台参考画面，非安装版证据；安装版实际退出已另行记录。
- `03_raw_modal_cancel.png`：RAW 转 JPG 弹窗参考画面，非安装版证据。
- `04_compress_modal_cancel.png`：批量压缩弹窗参考画面，非安装版证据。
- `05_escape_close.png`：设置/弹窗关闭参考画面，非安装版证据。

所有图片均来自脱敏 UI 画面，不含真实客户照片、头像、RAW、客户资料、完整路径或生产数据。元数据明确标注 InstalledUiVerified=false。

## 四条 Golden Path

- LocalSplit：RealFileVerified=true；InstalledUiVerified=false；磁盘输出 6；Task Center=Completed；结果一致。
- RawToJpeg：RealFileVerified=true；InstalledUiVerified=false；磁盘输出 1；Task Center=Completed；结果一致。
- BatchCompress：RealFileVerified=true；InstalledUiVerified=false；磁盘输出 3；Task Center=Completed；结果一致。
- Collage：RealFileVerified=true；InstalledUiVerified=false；磁盘输出 1；Task Center=Completed；结果一致。

## 交互门禁

modal_interaction_verified: false
tutorial_exit_verified: false（代码、自动化和一次独立安装版点击已确认；完整安装门禁仍待用户前台复核）
tutorial_step18_verified: false
escape_close_verified: false

四项门禁只有在完整安装版前台验收完成后才可改为 true；本轮不满足 RC3 条件。

## 未验证项目

- RAW 转 JPG、批量压缩、拼图和归片弹窗的安装版真实取消后，Modal 与 Task Center 的一致性。
- Step18 人为制造缺失报告后，安装版重试、返回上一步和退出教程。
- 所有页面弹窗的安装版 Esc 关闭，以及关闭后底层页面恢复输入。
- 日历右键、关闭档期重启持久化、在线选片四标签和结果同步。
- 用户前台四条 Golden Path 的完整操作。

## 最近重构报告

以下五份报告在产品仓库中均存在，且基线提交可追溯；它们是历史快照，不代表当前 hotfix HEAD：

- `回传给GPT_像素蛋挞_ProductRedesign_RC3核心可用性抢救报告.md`：存在，基线 e606139。
- `回传给GPT_像素蛋挞_ProductRedesign_RC2报告.md`：存在，基线 ca6924e5。
- `回传给GPT_像素蛋挞_在线选片V1桌面端与微信小程序规划报告.md`：存在，规划报告。
- `回传给GPT_像素蛋挞_产品重构总验收报告.md`：存在，基线 6443d3d。
- `回传给GPT_像素蛋挞_产品体验总重构报告.md`：存在，基线 6443d3d。

## 请求GPT审查

请审查本轮根因、统一 Modal 合同、教程退出/Step18 修复、测试结果、安装包哈希和脱敏证据。请保持 `UserVerified=false`、`InstalledUiVerified=false`，在用户完成前台验收前不要批准 RC3。
