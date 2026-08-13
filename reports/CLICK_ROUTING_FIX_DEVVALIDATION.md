# Click Routing Fix DevValidation

Protocol: `pixel-tart-handoff/v1`

## 结论

旧用户物理会话证明鼠标 Down/Up 已到达教程退出按钮，但标准 `Button.Click` 未生成。源码机制与八次重复尝试共同支持一个高置信根因：教程布局刷新在按下期间夺回焦点，使 Release 模式按钮在 MouseUp 前失去按压/捕获状态。

旧诊断没有记录 Mouse Capture、IsPressed 或按钮实例 ID，因此这些中间状态保持“未直接记录”，不作为用户实测结论。

## 修复范围

- 仅 Escape 类控件使用 PointerDown：教程退出、教程 X、Shell/Modal/Drawer 关闭 X。
- 普通保存、删除、转换、复制和导出按钮不变。
- 键盘和 UI Automation 的标准 Click 路径保留。
- 同一输入事件只分发一次。
- 教程布局在鼠标按下期间不再强制夺焦。
- 物理诊断可在 PointerDown 成功关闭时关联同一真实物理 attempt，且 UIA/Command 不能冒充。

## 门禁

- Debug：2017/2017。
- Release：2017/2017。
- 新包：`artifacts/releases/2.3.0/installer/PixelTart_2.3.0_ClickRoutingFix_DevValidation_x64.exe`。
- SHA256：`F9870E01E4B7CC763B68FB2DC1992B580D97222C7BA1488F9BE66CBCE0A042DC`。
- ProductVersion：2.3.0；SchemaVersion：5。

## 用户待验收

1. 真实鼠标点击教程“退出教程”。
2. 真实鼠标点击教程唯一 X。
3. 拼图右上角仅一个 X，真实鼠标点击后返回。

`UserVerified=false`，新包的物理路径均保持 pending。

## 安全

公开 Handoff 未包含原始 Session、坐标、本机路径、日志、截图、客户资料、照片、RAW、数据库、Key 或 Token。
