# Physical Pointer Diagnostic DevValidation2 启动入口验收

## 结果

- 桌面快捷方式：通过
- 开始菜单入口：通过
- 安装完成页启动：通过
- 桌面快捷方式经 Windows Shell 再次启动：通过
- 窗口标题“像素蛋挞 [Physical Pointer Diagnostic]”：通过
- Windows 已安装应用名称：通过
- 独立 Acceptance 数据根：通过
- 正式版 LocalAppData 未变化：通过

## 安装包

- Build Type: PhysicalPointerDiagnostic_DevValidation2
- ProductVersion: 2.3.0
- FileVersion: 2.3.0.0
- Size: 50749979 bytes
- SHA-256: 1A8A481E06A05E5C5C3A9279860862944EC4AC7ABDF73C36EB0BC1220ED562ED

## 安全边界

新包使用独立 AppId、安装目录和 Acceptance 数据根。快捷方式只指向本诊断版 Acceptance EXE。安装、启动和验收未覆盖、卸载、关闭或修改正式版，也未上传截图、客户资料、照片、RAW、数据库、日志、密钥或本机绝对路径。

## 仍未验证

本报告只证明安装器入口可用。Physical Pointer 功能仍需用户真实前台验证，因此 `PhysicalPointerVerified=false`、`UserVerified=false`。
