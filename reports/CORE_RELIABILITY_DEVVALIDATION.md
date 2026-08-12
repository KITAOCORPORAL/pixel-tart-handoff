# Pixel Tart CoreReliability DevValidation

## 安全边界

- 所有输出均位于独立 `PixelTart_Validation` 临时目录。
- 单个 ARW 样本仅只读使用；源文件未修改、未移动、未删除、未上传、未提交。
- 分片、批量压缩和拼图使用独立测试副本，不覆盖真实原片。
- 本报告不包含完整路径、客户资料、照片内容、RAW 内容、未脱敏日志或凭证。

## 四条 Golden Path

| 路径 | RealFileVerified | InstalledUiVerified | 磁盘输出数量 | Task Center 终态 | 结果一致 |
| --- | --- | --- | ---: | --- | --- |
| 本地分片 / 归片 | true | false | 6 | Completed | true |
| RAW → JPG | true | false | 1 | Completed | true |
| 批量压缩 | true | false | 3 | Completed | true |
| 拼图 | true | false | 1 | Completed | true |

安装包已启动并使用独立运行目录；前台自动化检测到用户输入后停止，因此 `InstalledUiVerified` 只由用户亲自完成后更新。

## 测试门禁

Debug 与 Release 均为 1952/1952 通过，0 失败、0 跳过、0 错误；DPI 为 101/101（Debug 与 Release）。

## 当前决定

保持 DevValidation，不生成 RC3，不进入新功能开发。`UserVerified=false`。
