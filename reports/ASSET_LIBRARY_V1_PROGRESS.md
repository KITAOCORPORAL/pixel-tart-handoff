# Asset Library V1 Progress

Protocol: `pixel-tart-handoff/v1`

Status: Development Preview / waiting for review

Branch: `feature/asset-library-v1`

HEAD: `6d93a8aec0c712224a1b1094f61eb4be2299567a`

Baseline: `4dac5f8e4460b7a67309646b6133bd186c121fea`

## 已完成

- 建立独立 worktree 与分支，未修改 P0 产品分支。
- 新增稳定 `AssetItem.AssetId` 与独立 SQLite metadata repository。
- Reference Mode 默认不移动、不改名、不删除源文件。
- Managed Copy 必须显式指定目标目录，源文件保持不变。
- Folder、Subfolder、Asset-Folder 多对多 membership。
- Tag、Tag Group、Asset-Tag 多对多 membership。
- 批量 membership、Tag merge、评分/备注与进程内 Undo。
- Smart Folder 基础 AND/OR/NOT、精确/包含/不等、数值与 Regex；错误 Regex 返回可读错误。
- 未分类、无标签、搜索、筛选和有界分页。
- 独立三栏 WPF 开发预览；F 聚焦分类入口，Shift+D 重复上一次 Folder 分类。
- Recycling 虚拟化列表、DecodePixelWidth 缩略图与 128 项有界缓存。
- 100,000 条 synthetic metadata 分页 smoke；不创建真实图片、RAW 或 SQLite BLOB。

## Schema

Proposal: `docs/architecture/AssetLibrarySchemaProposal.md`

正式 SchemaVersion 保持 5。没有注册 Schema 6，没有打开或迁移正式产品数据库。`SelectionAsset.Id`、`TetherAssetRecord.Id` 和旧 Media index 身份没有被错误合并成全局 AssetId。

## 测试

- Asset Library Core: 10/10 passed
- Asset Library WPF Preview: 7/7 passed
- Preview Debug build: 0 warning / 0 error
- Full solution Debug build: 0 warning / 0 error
- Full solution Release build: 0 warning / 0 error

## 安全边界

- 没有 Eagle 品牌、Logo、图标、素材、页面皮肤或文案复制。
- 没有 P0 Input/Tutorial/Shell Escape 文件改动。
- 没有安装器、Release、Tag、main merge 或 P0 merge。
- 没有客户照片、RAW、数据库、日志、密钥或本机绝对路径进入 Handoff。

## 延后

- 正式数据库 migration 与受控合并。
- 真实 100,000 图片库的滚动、内存和缩略图取消基准。
- 多列虚拟化网格、持久化 Undo Journal、完整拖拽/Tag Manager UI。
- AI、语义搜索、以图搜图、云同步、浏览器扩展和插件系统。

UserVerified: false
