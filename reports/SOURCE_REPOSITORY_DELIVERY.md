# 像素蛋挞私有源码仓库交付

- 交付日期：2026-08-18（Asia/Shanghai）
- 私有源码仓库：<https://github.com/KITAOCORPORAL/pixel-tart-source-private>
- 可见性：**Private**（已在 GitHub 登录页面直接确认）
- 默认分支：`main`
- 交付性质：源码与可复现构建资料交付，不代表产品发布、RC 生成、P0 合并或用户验收完成。

## 1. 分支与提交

原始本地仓库没有远程地址。交付前对指定分支及共同祖先执行了历史级安全过滤，因此 GitHub 中的提交 SHA 与本地原始 SHA 不同；原始仓库和 worktree 未被改写。

| 交付分支 | GitHub 实际 HEAD | 原始本地提交 / 来源 | 对应说明 |
| --- | --- | --- | --- |
| `main` | `3d6225f40b335ded39e535a5eb01d7c967a180be` | `1c9f8e7a7334b29b0081a1ac83faf55882bf6b46` → 安全过滤基线 `b0d243488a43e8859e2c2e97df28dbceb71ef323` | 默认稳定分支；HEAD 仅额外增加 `AGENTS.md` 与 `SECURITY.md` |
| `feature/pixel-tart-product-redesign` | `1096ae3353d0b875e498d80c2f491c242240cc0b` | `4dac5f8e4460b7a67309646b6133bd186c121fea` | P0 / 产品重构 |
| `feature/modular-harness-v1` | `bd904cd21e2668f4aa381181f5dcfad7832fdb5b` | `5be53f6393bba1069e921476bff976257d4f8505` | Modular Harness V1 |
| `feature/asset-library-v1` | `189bf6e6b5b8a523d95bf28eab3cd8f10ece5c16` | `3da45d53e4628a743a2809f908cbdfd60d706c43` | Asset Library V1 |
| `feature/online-selection-v1` | `349bf4006faf9b80a3691b7d9fd368271ca58ebd` | `e30eac4762af7eff837645a8303c47eeb95c5fe2` | Online Selection V1 |
| `handoff/source-snapshot-20260818` | `fd4277020ea78e4b273208ca5a0f36e8383b090f` | 分离 worktree 中 7 个未提交的测试文件改动 | SQLite 连接池隔离的可审计快照；未改动原 worktree |

四个报告 SHA 均已用本地 Git 对象数据库验证为真实提交，分支名和 HEAD 与报告一致。它们没有被提前合并；交付仓库继续保留独立分支边界。

## 2. 源码覆盖与数量

六个交付分支的唯一并集包含：

- 755 个受 Git 跟踪的文件；
- 703 个文本源码、项目/构建配置或文档文件；
- 其中 146 个测试/验收文件，557 个非测试文本源码、配置或文档文件；
- 1 个 Solution、27 个 `.csproj`、23 个数据库/Schema/迁移相关源码或文档文件。

已确认包含 WPF Shell、工作台、归片、日历、联机拍摄、收支、项目历史、RAW 转 JPG、压缩、水印、拼图、文件整理、SchemaVersion 5、迁移与回滚、Modular Harness、Asset Library、Online Selection Server/LocalDev/契约、验收工具及 `clients/wechat-mini-program/`。`Directory.Build.props`、版本配置、构建脚本和 Inno Setup 配置存在；原始仓库没有 `global.json`、`Directory.Build.targets` 或 NuGet 配置文件，未伪造这些文件。

## 3. 克隆、构建与测试

已从 GitHub 私有地址重新克隆到全新目录。远端默认分支解析为 `main`，六个交付分支均可读取，无 Git submodule，`git fsck` 未发现不可达交付对象或对象错误。

验证环境：Windows 10 x64（10.0.19045）、.NET SDK 10.0.302、Microsoft.WindowsDesktop.App 10.0.10、Node.js 24.15.0。

| 分支 | Restore / Debug build | 测试结果 |
| --- | --- | --- |
| `main` | 通过；0 警告、0 错误 | Core 768/768、WPF 61/61 通过；DPI 证据套件 12 通过、26 失败，失败均为被禁止上传的 `artifacts/automated-dpi-review` JSON 不存在 |
| P0 | 通过；0 警告、0 错误 | Core 1136/1136、WPF 780/780 通过 |
| Modular Harness | 通过；0 警告、0 错误 | Harness 14/14 通过；Core 1187 通过、1 失败（隔离测试仍断言旧的精确异常文本）；WPF 791 通过、1 失败（依赖已排除的 UI 证据 PNG） |
| Asset Library | Solution 与验收运行器独立 restore 均通过；Debug build 通过，0 警告、0 错误 | Core/验收 1190 通过、0 失败、2 跳过；WPF 793/793 通过。两项跳过为非 sRGB ICC 参考和 RAW 内嵌预览参考 |
| Online Selection | 通过；0 警告、0 错误 | Selection API 10/10、Core 1145/1145、WPF 782/782、小程序契约 1/1 通过 |
| source snapshot | 通过；0 警告、0 错误 | Core 693/693、WPF 39/39 通过 |

结论：`source_clone_verified=true`、`source_build_verified=true`。由于 Modular Harness 仍有 1 个真实断言失败，且证据型套件因安全排除的生成物无法全绿，`source_tests_verified=false`；没有把部分通过写成整套测试通过。

## 4. 安全扫描

- 使用从官方发布页取得并校验校验和的 Gitleaks 8.30.1 扫描原始完整可达历史与脱敏交付历史。
- 原始历史扫描 136 个提交、约 7.70 MB；最终交付历史扫描 109 个提交、约 6.68 MB。
- 共出现 24 个 `generic-api-key` 命中，逐项确认全部来自 `Spacing.xaml` 和三份颜色资源 XAML 的 `x:Key`/十六进制色值，是确定的设计令牌误报；有效凭据命中为 0。
- `main` 最终交付文档提交复扫为 0 命中。
- 微信小程序配置为 `touristappid`、空 `devAccessToken`、`127.0.0.1`；授权配置中的公钥和验证 token 均为空。
- 完整交付历史中未发现本机绝对路径；最大 Git blob 约 151 KB，没有来源不明的大文件。
- 图像并集为 15 个源资产（11 个 PNG、4 个 SVG）：应用图标、品牌图、占位图和一张经目检确认的纯插画工作台封面；没有人物照片、客户照片、JPG 原片或 RAW。
- 未发现数据库、SQLite 实例、日志、崩溃转储、Cookie、浏览器数据、证书、私钥、安装包、压缩包、PDB 或对象存储内容。

## 5. 安全排除范围

从交付历史中排除了：

- `artifacts/`、`ui-review/`、验收报告/诊断 Session、临时迁移输出和外部参考素材目录；
- `bin/`、`obj/`、`.vs/`、`node_modules/`、`publish/`、`TestResults/`；
- EXE/MSI/MSIX/APPX、ZIP/7Z、PDB、日志、转储、SQLite/数据库实例；
- `.env`、密钥/证书、授权实例、客户数据、客户照片、JPG/RAW 原片；
- 本机绝对路径、用户名及 LocalAppData/AppData 内容。

安装器源码和配置保留，但生成的安装包、证书和签名材料未上传。公开交接仓库中原有的脱敏 UI 证据继续保留；没有把这些生成物复制进私有源码仓库。

## 6. 尚未上传或无法验证

- 未上传任何生成的发布包、安装包、签名材料、运行数据库、日志、缓存、缩略图、代理图或验收证据目录。
- DPI/截图元数据测试需要本地重新生成被排除的证据，因此不能在纯源码克隆中宣称通过。
- Modular Harness 的精确异常文本断言仍有 1 项失败，属于交付时现状，未在本轮修改业务代码或测试。
- 未验证正式微信 AppID/服务器、付费授权、相机硬件/SDK、签名证书、物理多屏验收或外部插件运行时。
- `P0Merged=false`、`RCGenerated=false`、`UserVerified=false`、`ExternalPluginRuntime=false` 保持不变；源码上传不等于产品发布。

## 7. 下一次 Codex 继续指令

1. 克隆 `https://github.com/KITAOCORPORAL/pixel-tart-source-private.git`，先阅读根目录 `AGENTS.md`、`SECURITY.md` 和本报告。
2. 根据任务切换到对应功能分支；不要为方便而提前合并 P0、Harness、Asset Library 或 Online Selection。
3. 先确认 `git status` 干净并复查完整历史安全扫描，再运行 restore/build/test。
4. Asset Library 测试前额外 restore `tools/AssetLibraryV16Acceptance/PixelTart.AssetLibrary.V16.AcceptanceRunner.csproj`；单独运行 WPF 测试时使用 x64 平台。
5. 需要评估 7 个未提交测试改动时，只审阅 `handoff/source-snapshot-20260818`，不要直接修改或假定其应合并。
6. 生成的验收证据、数据库、日志、安装包和客户媒体必须继续留在 Git 之外。
