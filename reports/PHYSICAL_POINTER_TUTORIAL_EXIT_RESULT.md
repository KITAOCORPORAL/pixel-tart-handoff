# Physical Pointer Tutorial Exit 真实结果

## 会话

- Diagnostic Session: `PT-INPUT-20260813-003`
- 对应尝试：`pointer-009`
- 诊断捕获：成功

## 四层结果

- Win32：WM_LBUTTONDOWN=true，WM_LBUTTONUP=true
- WPF：PreviewMouseLeftButtonDown=true，PreviewMouseLeftButtonUp=true，Handled=false
- OriginalSource：TextBlock
- Source：Button[AutomationId=TutorialExitButton]
- InputHitTest：TutorialExitButton 模板内 TextBlock
- Visual HitTest：TutorialExitButton 模板内 TextBlock
- Blocking Element：not_recorded；实际命中父链经过 TutorialExitButton
- Blocking Ancestor：null；命中父链未记录到禁用或不可命中的祖先
- TutorialExitButton Click：false
- ForceExitTutorial Entered：false
- Tutorial Overlay Detached：false
- Tutorial Active After Click：true
- Backdrop Removed：not_recorded
- Sidebar Restored：not_recorded
- Workbench Restored：not_recorded

## 结论

用户真实物理点击已穿过 Win32 与 WPF Preview 层，HitTest 也落在 `TutorialExitButton` 内部。Session 没有独立的 Blocking Element 字段，不能据此扩大声明为诊断器排除了所有可能遮挡；可以确认的是命中父链中没有记录到禁用或不可命中的祖先。失败断点位于 Preview Down/Up 完成后、WPF Button Click 生成之前。因此 `physical_pointer_verified=false`、`tutorial_exit_physical_verified=false`。

诊断初始化、Win32 Hook、WPF AddHandler 和 Session Writer 均正常；这不是 `DIAGNOSTIC_CAPTURE_FAILED`。

本报告只包含脱敏结论，不包含原始 Session、坐标、本机路径、截图、客户资料、照片、RAW、数据库或日志。
