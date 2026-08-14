# Codex → GPT Handoff

Protocol: pixel-tart-handoff/v1
ReportId: parallel-feature-v1-20260814
CreatedAt: 2026-08-14T10:31:01+08:00
Project: Pixel Tart

## Git
Branch: feature/asset-library-v1 + feature/online-selection-v1
HEAD: asset=6d93a8aec0c712224a1b1094f61eb4be2299567a; online=4b6ad237acc15c95624f9b1370d3a176c53d9e20
WorkingTreeClean: true

P0 baseline branch: `feature/pixel-tart-product-redesign`

P0 baseline HEAD: `4dac5f8e4460b7a67309646b6133bd186c121fea`

P0 merge status: **not merged**。两个功能分支保留在独立 worktree，没有合并到 P0、main，也没有创建 Tag 或 RC。

## Version
ProductVersion: 2.3.0
FileVersion: 2.3.0.0
SchemaVersion: 5

本轮只生成两份 Schema Proposal，未注册 Schema 6，未访问或迁移正式产品数据库：

- `docs/architecture/AssetLibrarySchemaProposal.md`
- `docs/architecture/OnlineSelectionSchemaProposal.md`

## 本轮完成

Asset Library V1 开发预览：稳定 `AssetId`、Reference/显式 Managed Copy、虚拟 Folder/Tag/Tag Group 多对多关系、批量 membership、Tag merge 与 Undo、Smart Folder 基础规则、Regex 错误处理、未分类/无标签查询、搜索与分页、F 分类入口、Shift+D 重复分类、三栏独立 WPF 预览、真实 Recycling 虚拟化列表及 128 项有界缩略图缓存。100,000 条 synthetic metadata 分页 smoke 已通过；未生成或提交图片/RAW。

Online Selection V1：在现有 `SelectionProject`/`SelectionAsset`/`SelectionRule`/`IOnlineSelectionProvider` 上收口；增加可选 `SourceAssetId`、本地 Choice/Favorite/Comment Mock、版本化 `FinalSelectionSnapshot`、确认/重开/锁定、TXT/CSV UTF-8 无 BOM 导出、Upload Queue 状态/重试、结果同步契约。桌面仍固定四个标签页。

Server/API：新增独立 Local Dev contract 与 `ISelectionObjectStorage` 本地实现；不在 WPF 进程启动 listener，不包含生产域名、凭证或云配置。

WeChat Mini Program：`clients/wechat-mini-program/` 仅包含 Project、Gallery、Photo、Selected、Confirm 五页；所有网络调用集中在 `services/api.ts`，状态集中在 SelectionStore。当前仅 Mock/Local Dev，不包含 AppSecret、SessionKey 或长期 Token，不宣称已经上线。

本轮未修改 P0 Input/Tutorial/Shell Escape 文件、安装器、正式发布物或公开用户数据。

## 测试
Debug: 84/84 focused passed，0 failed，0 skipped；两个分支完整 solution Debug build 均为 0 warning / 0 error。

- Asset Library Core: 10/10
- Asset Library WPF Preview: 7/7
- Online Selection Core: 41/41
- Online Selection WPF: 23/23
- Selection API: 3/3

Release: 两个分支完整 solution Release build 均为 0 warning / 0 error；本轮未把开发预览包装为正式 Release。

DPI: 本轮未单独运行 DPI 验收；P0 基线与其验收文件未修改。

## Installed UI
CodeVerified: true
AutomatedVerified: true
InstalledUiVerified: false
UserVerified: false

两个功能均为开发预览，没有安装版前台用户验收。自动构建、代码审计和合成 metadata 测试不能替代用户确认。

## 安装包
Path:
SHA256:

本轮没有生成或覆盖任何 DevValidation、RC 或正式安装包。

## UI证据
未向公开 Handoff 上传截图、客户照片、RAW、数据库、日志、路径凭证或任何私有素材。Asset Library 使用独立预览宿主；Mini Program 仅提交代码级 Mock 原型。

## 未验证项目

1. Asset Library 在真实 100,000 图片库上的首屏、滚动、缩略图取消与内存基准；当前只完成 100,000 条 metadata smoke。
2. Asset Library 多列虚拟化网格、持久化 Undo Journal、完整拖拽 Folder/Tag Manager 交互。
3. Online Selection 的生产云、HTTPS、数据库、对象存储、正式微信 AppID/域名与服务端 `code2Session`。
4. 微信开发者工具和真实设备上的五页联调、弱网恢复与多设备版本冲突。
5. 两个功能分支与 P0 的受控合并、完整回归和前台用户验收。

## 请求GPT审查
请审查稳定身份边界、Schema Proposal、源文件非破坏性、Folder/Tag/Smart Folder 关系、Selection `SourceAssetId` 引用、Proxy/隐私边界、API 路由一致性、微信凭证边界及 P0 隔离。保持 `state=waiting_for_gpt_review` 与 `UserVerified=false`，在 P0 真实交互问题完成前不要合并或发布。
