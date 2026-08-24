# Third-Party Notices / 第三方声明

This file records how LineTamer relates to third-party projects. LineTamer itself is
MIT-licensed; the components below were **not** copied line-by-line, but several
implementations were derived from the *approach* and *technical facts* of upstream
projects, so attribution is recorded here per open-source good practice.

本文件记录 LineTamer 与第三方项目的关系。LineTamer 本身采用 MIT 许可证；以下条目并非
逐行复制，但部分实现源自上游项目的**思路与技术事实**，按开源惯例在此注明出处。

## Derived implementations / 衍生实现

| 文件 / 组件 | 衍生自 | 内容 |
| --- | --- | --- |
| `hooks/HomeTabHider.java` | **LineXtra 1.0.1** (yagiyuu) | 底栏标签隐藏：hook `MainActivity.onCreate` 后遍历 `main_tab_container` 子项，按资源条目名前缀 `bnb_*` 折叠。资源条目名是 LINE 资源事实（非版权表达）；代码为本项目重新实现 |
| `hooks/AdViewBlocker.java` | **LIME v1.12.1** + **LineXtra 1.0.1** | 广告视图折叠锚点（SmartChannel / LadAdView / addView 扫描）与「构造器持久重隐藏」思路 |
| `hooks/CallTone.java` | **LIME v1.12.1** (Ringtone) | 来电铃声触发条件：sync 响应 toString 含 `type:NOTIFIED_RECEIVED_CALL` |
| `hooks/SyncGuard.java` / `hooks/ThriftGate.java` | **LIME v1.12.1** | 防撤回/防已读/遥测抑制的 Thrift 方法名集合（方法名为协议常量，属技术事实；26.11.0 已重新校准） |
| `hooks/DarkModeHooks.java` | **LIME v1.12.1** (思路) | 子设备深色主题解锁思路（实现与目标类均为本项目独立校准） |

Upstream links / 上游链接:

- LIME: https://github.com/Chipppppppppp/LIME — **MIT License, Copyright (c) 2024 Chippppp**
  （已通过 GitHub API 确认；MIT 要求保留版权声明，本条即满足）
- LineXtra: 无 GitHub 仓库（`yagiyuu/LineXtra` 返回 404），仅 [LSPosed 模块页](https://modules.lsposed.org/module/io.github.yagiyuu.linextra/)
  存有发布包，页面未展示任何许可证声明 → 视为**保留所有权利**：仅引用其公开的资源条目名
  等事实，不逐行复制其代码，也未分发其任何文件。

## API stubs / 编译桩

- `build-stub/de/robv/android/xposed/*`：Xposed 框架（rovo89/LSPosed，Apache-2.0）公开 API 的
  手写签名桩（仅方法签名，无实现）。签名重现属 API 兼容用法。
- `build-stub/android/*`、`build-stub/androidx/*`、`build-stub/kotlin/*`：Android / Kotlin
  标准库公开 API 的手写签名桩。

## 注意事项 / Maintainer checklist

- 上游许可证状态已确认（2026 复核）：**LIME = MIT**（保留版权声明即可，见上）；**LineXtra =
  无仓库、无许可证声明**（视为保留所有权利）→ 仅引用事实（bnb_* 资源条目名、类名），
  保持「重新实现」形态，不逐行复制、不随本仓库分发其文件。
- LINE 官方 APK、LIME APK、LineXtra APK 等第三方二进制**不得提交到本公开仓库**（仅存在于
  工作区素材目录）。
- 本模块仅供学习与研究 Android Hook 技术使用，请勿用于商业用途。
