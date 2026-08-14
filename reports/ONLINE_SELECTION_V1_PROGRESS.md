# Online Selection V1 Progress

Protocol: `pixel-tart-handoff/v1`

Status: Desktop/Server/Mini Program Local Development Preview

Branch: `feature/online-selection-v1`

HEAD: `4b6ad237acc15c95624f9b1370d3a176c53d9e20`

Baseline: `4dac5f8e4460b7a67309646b6133bd186c121fea`

## Desktop 已完成

- 复用并收口现有 `SelectionProject`、`SelectionAsset`、`SelectionRule` 与 `IOnlineSelectionProvider`，没有建立第二套领域模型。
- `SelectionAsset.Id` 继续作为项目内稳定 `SelectionAssetId`，新增可选 `SourceAssetId` 与文件名/Stem snapshot。
- 项目列表与固定四标签页：照片、客户选片、设置、交付结果。
- RAW 只生成 Proxy JPG；RAW 不进入上传队列。
- Proxy 默认 2560 long edge、Quality 85、sRGB，并保持不复制 GPS、完整 EXIF、本地路径或用户名的边界。
- Local Choice/Favorite/Comment Mock、进度、确认、版本化 `FinalSelectionSnapshot`、锁定与重新开放。
- TXT/CSV UTF-8 无 BOM 结果导出。
- Upload Queue 状态、失败重试、暂停/恢复契约和结果同步到归片的现有安全链。

## Server/API

- 新增独立 `PixelTart.SelectionApi.Server` Local Dev project。
- 新增本地 `ISelectionObjectStorage` 实现与 API route contract。
- Canonical route 保持现有 `/v1/...`，没有并存第二套 `/api/v1` 路由。
- Server contract 默认 `StartsListener=false`、`IsProductionConfigured=false`。
- WPF 进程不启动本地 HTTP listener，不包含生产数据库或云凭证。

## WeChat Mini Program

目录：`clients/wechat-mini-program/`

仅包含五页：

1. Project
2. Gallery
3. Photo
4. Selected
5. Confirm

页面不直接写 `wx.request`；统一由 `services/api.ts` 处理 request/retry/error mapping，SelectionStore 保存 Project、Assets、Choices、Favorites、Comments、Confirmed 与 PendingRequests。

当前为 Mock/Local Dev。`Provider=None`；没有 AppSecret、SessionKey、长期 Token、手机号或真实身份数据。正式登录必须由服务端处理 `wx.login` 临时 code 与 `code2Session`，并使用 HTTPS 和微信后台合法域名。

## Schema

Proposal: `docs/architecture/OnlineSelectionSchemaProposal.md`

正式 SchemaVersion 保持 5。桌面开发预览继续使用现有 JSON workspace；没有污染 P0 数据库或注册正式 migration。

## 测试

- Online Selection Core: 41/41 passed
- Online Selection WPF: 23/23 passed
- Selection API: 3/3 passed
- Full solution Debug build: 0 warning / 0 error
- Full solution Release build: 0 warning / 0 error

## 未上线与延后

- 生产域名、HTTPS、对象存储、服务端数据库、部署环境和正式密钥。
- 微信 AppID、合法域名配置、真实 `code2Session` 和设备联调。
- 生产云上传、发布、撤销与多设备并发冲突处理。
- 与 Asset Library 分支和 P0 分支的受控合并。

OnlineSelectionProvider: None

MiniProgramProduction: false

UserVerified: false
