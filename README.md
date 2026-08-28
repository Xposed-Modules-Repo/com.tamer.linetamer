# LineTamer（LINE 净化 / LINE クリーナー）

> ## ⚠️ 本模块由 AI 生成 / This module is AI-generated / AI 生成
> 本项目由大语言模型（AI）在人类指导下生成并迭代维护，包括全部 Hook 代码、设置界面、
> 构建流水线与文档。代码未经人工长期审计，请自行评估风险后使用；欢迎人工审查与 PR。
>
> This project was generated and iterated by a large language model (AI) under
> human direction — all hook code, the settings UI, the build pipeline and these
> docs. The code has not been long-term audited by humans; evaluate the risk
> yourself. Human review and PRs are welcome.
>
> このプロジェクトは人間の指示のもと大規模言語モデル（AI）が生成・継続改良しています。
> コードは長期間の人間監査を受けていません。利用は自己責任でお願いします。

面向 **LINE**（`jp.naver.line.android` 26.11.0 / 26.13.0，持续跨版本适配）的功能精简 LSPosed 模块。
设计原则：聊天与通话永远可用；其余功能全部提供独立开关，可随时屏蔽/恢复。

An LSPosed module that trims **LINE** (`jp.naver.line.android` 26.11.0 / 26.13.0, cross-version support is ongoing).
Design principle: chat & calls always keep working; every other feature is an
independent switch you can toggle on or off.

**LINE**（`jp.naver.line.android` 26.11.0 / 26.13.0、継続的にクロスバージョン対応）向けの機能削減 LSPosed モジュールです。
トークと通話は常に使えます。その他の機能はすべて独立スイッチで個別に切り替えられます。

## 功能一览 / Features / 機能一覧

| 分类 Category 分類 | 功能 Feature 機能 |
| --- | --- |
| 主页 Home ホーム | 主页整页净化：底栏只留聊天 tab，原顶栏功能下放到底栏功能区（头像昵称→个人资料、服务、通知、设置，直开 LINE 对应页）；昵称可一键隐藏（隐私）；内容网络拦截：getHomeServiceList/getInstantNews 请求直接不发 / home fully purified: chat is the only tab left, top-bar functions move into the bottom-bar area (avatar→Profile, Services, Notifications, Settings via explicit intents); nickname can be hidden for privacy; getHomeServiceList/getInstantNews never sent / ホーム完全浄化：トークタブのみ残し、上バー機能は下バー機能エリアへ（プロフィール/サービス/通知/設定）；ニックネームは非表示化可能（プライバシー） |
| 底栏 Tabs タブ | 主页净化开启时除聊天外全部隐藏（News / Wallet / Shopping / Mini-Apps 等） / with home purification on, every tab except chat is hidden (News, Wallet, Shopping, Mini-Apps, …) / ホーム浄化オン時はトーク以外のタブ（News・Wallet・Shopping・Mini-Apps 等）をすべて非表示 |
| 广告 Ads 広告 | 请求源头拦截（getBanners 零流量）+ 广告视图本地折叠 / ad requests blocked at source + ad views collapsed / 広告リクエストを送信元で遮断（getBanners ゼロ通信）＋広告ビューを折りたたみ |
| 骚扰 Nuisance 迷惑 | 来电本地补响铃声 / local ringtone on incoming call / 着信時にローカルで着信音を再生 |
| 实验 Experimental 実験 | 防撤回 / 永不回执已读 / 遥测抑制 / 子设备深色主题解锁 / anti-unsend / read receipts never sent / telemetry suppression / dark theme unlock on sub-device / 消去防止 / 既読を送信しない / テレメトリ抑制 / サブ端末のダークテーマ解放 |
| 界面 UI | 设置页中/日/英三语；风险分级 [A]视图层 [B]协议层 [C]实验性 在每个开关注明 / trilingual settings (CN/JA/EN); risk class [A] view / [B] protocol / [C] experimental on every switch / 設定画面は中・日・英の三言語；各スイッチにリスク区分 [A]ビュー層 [B]プロトコル層 [C]実験的 を明記 |

v1.6.0 起主页净化 = **底栏版**：底栏只留聊天一个 tab（主页等其余 tab 全部隐藏，
聊天自动左对齐），原主页顶栏的功能下放到底栏右侧的功能区——头像+账户昵称
（contacts 库只读查询，可在设置里一键隐藏以保护隐私）打开个人资料，「服务 /
通知 / 设置」三枚照原顶栏重绘的图标，显式 Intent 直开 LINE 对应页面（类名取自
manifest 明文，不依赖混淆类名；通知中心 26.13.0 的新类名带 26.11.0 回退候选）。
功能区配色按底栏实际明暗自动适配（读底栏背景图像素、无监听；LINE 主题独立于
系统设置，另备手动浅色开关）、垂直居中对齐可见底栏（运行时测量锚点，适配任意
分辨率与手势条模式）。冷启动直接落在聊天页（对聊天 tab 注入合成触摸，由 LINE
自身控制器解析页面位置，免疫页序漂移）。网络层拦截同步保留：getHomeServiceList /
getInstantNews 请求直接不发——主页不可达后自然零流量。
Since v1.6.0 home purification = the **bottom-bar edition**: the chat tab is the
only tab left in the bottom bar (home and every other tab hidden, chat left-aligned),
and the former home top-bar functions move into a function area on the right of
the bar — avatar + nickname (read-only contacts DB query, hideable in settings for
privacy) opens your profile, and Services / Notifications / Settings icons (redrawn)
open LINE pages via explicit intents (26.13.0 names with 26.11.0 fallbacks). The
area follows the bar's actual light/dark appearance (auto-detected from the bar
background bitmap, no listeners; manual light override provided since LINE's
in-app theme is independent of the system setting), is vertically centered on the visible bar
(runtime-measured anchors, any resolution / gesture-bar mode), and cold starts land
directly on the chat page (a synthetic tap on the chat tab lets LINE's own
controller resolve the page position — immune to page-order drift).
Network blocking stays: getHomeServiceList / getInstantNews are never sent — with
home unreachable that is naturally zero traffic.
v1.6.0 からホーム浄化は**下バーエディション**：下バーにはトークタブだけを残し
（ホーム等の他タブはすべて非表示、トークは左寄せ）、旧ホーム上バーの機能は
下バー右侧の機能エリアへ——アバター＋ニックネーム（連絡先 DB を読み取り専用
照会、設定で非表示化できプライバシー保護）でプロフィール、サービス / 通知 /
設定の 3 アイコンは明示的 Intent で直接起動します。機能エリアは下バーの実際の
明暗を自動判定（下バー背景ビットマップから読み取り、リスナー常駐なし）して
配色し、可視バーに対して垂直中央揃え（実行時測定アンカー、解像度・ジェスチャー
バー不問）。コールドスタートは直接トークページに着地します。通信遮断
（getHomeServiceList / getInstantNews 送信せず）も継続です。

## 环境要求 / Requirements / 環境要件

* 已 root 设备：Magisk / KernelSU + Zygisk + **LSPosed**。
  A rooted device: Magisk / KernelSU + Zygisk + LSPosed.
  root済み端末（Magisk / KernelSU + Zygisk + LSPosed）。
* LINE **26.11.0**／**26.13.0**（两版逐字节校准，实机全量验证）。目标：跨版本持续适配。
  LINE **26.11.0** / **26.13.0** (both byte-calibrated, fully verified on device); cross-version adaptation ongoing.
  LINE **26.11.0**／**26.13.0**（いずれも実機検証済み）。継続的なクロスバージョン対応。

> 混淆类名随版本可能漂移，功能会静默失效而不崩溃。在 LSPosed 日志过滤 `LineTamer`
> 查看 `version check ok: 26.11.0` 与各 `armed` 行确认 Hook 命中。
> Obfuscated class names may drift across versions and hooks silently miss;
> filter LSPosed logs for `LineTamer` and check `version check ok` + `armed` lines.
> 難読化されたクラス名はバージョンごとに変わる可能性があり、機能が静かに効かなくなることが
> あります（クラッシュはしません）。LSPosed のログで `LineTamer` をフィルタし、
> `version check ok: 26.11.0` と各 `armed` 行を確認してフックが有効か確かめてください。

## 使用方法 / Usage / 使い方

1. LSPosed 中启用模块并勾选作用域「LINE」。
   Enable the module in LSPosed and select the "LINE" scope.
   LSPosed でモジュールを有効化し、スコープに「LINE」を指定。
2. 强制停止 LINE 后重新打开。
   Force-stop LINE and reopen it.  LINE を強制終了して再起動。
3. 设置入口：桌面图标；主页文字栏显示时也可长按文字栏 ≈1 秒。
   Settings: launcher icon; with the home top bar visible, long-press the bar (~1s).
   設定: ランチャーアイコン。ホームの文字バー表示中はバー長押し（約1秒）でも可。

> 权限说明 / Permissions note / 権限について：Hook 过程不执行任何 root 操作。仅设置页保存开关时可能请求
> root 同步一份全局配置副本；拒绝不影响功能，配置自动落到其它副本路径。
> No root operations while hooking; root is only requested when saving switches to
> sync a global config copy — denying it falls back automatically.
> フック中に root 操作は行いません。設定画面でスイッチを保存するときだけ root を要求し、
> グローバル設定のコピーを同期します。拒否しても機能は壊れず、設定は自動的に他の場所へ
> フォールバックします。

## 从源码构建 / Build / ビルド

不依赖 Gradle / Android SDK（手写 stub + 二进制 AXML/arsc + apksig）：
No Gradle / Android SDK dependency (hand-written stubs + binary AXML/arsc + apksig).
Gradle / Android SDK には依存しません（手書きスタブ＋バイナリ AXML/arsc＋apksig）。

```
python tools/build_module.py
```

流程：`javac --release 8`（stub）→ dalvik-dx → 手写 manifest（axml_writer）+ 最小 arsc
（arsc_builder）→ zip → apksig 签名。产物在 `dist/`。签名密钥不随仓库分发（密码在
gitignored 的 `tools/signing.local`，或用 HMT_KEYSTORE/HMT_KS_PASS 环境变量指定）。
构建深坑见 [PITFALLS.md](PITFALLS.md)。

Pipeline: plain javac against hand-written stubs → dalvik-dx → hand-written binary
manifest + minimal arsc → zip → apksig. Output lands in `dist/`. Build lessons:
[PITFALLS.md](PITFALLS.md).

手順: 手書きスタブに対して `javac --release 8` → dalvik-dx → 手書きバイナリ manifest
（axml_writer）＋最小 arsc（arsc_builder）→ zip → apksig で署名。成果物は `dist/` に
出力されます。署名キーはリポジトリに含めません（パスワードは gitignore された
`tools/signing.local`、または環境変数 HMT_KEYSTORE/HMT_KS_PASS で指定）。
ビルドの落とし穴は [PITFALLS.md](PITFALLS.md) を参照してください。

## 项目结构 / Layout / 構成

```
app/src/main/java/com/tamer/linetamer/
├── MainHook.java            # Xposed 入口，逐钩子隔离 / entrypoint, per-hook isolation / エントリポイント、フックごとに分離
├── LineConfig.java          # 开关键 + 配置三副本读取 / keys + 3-copy config loading / スイッチ定義＋設定の3重読み込み
├── hooks/                   # 功能实现 / feature hooks / 機能フック群
│   ├── ThriftGate           # Thrift 漏斗：广告拦截/已读/遥测/防撤回 / Thrift funnel: ads/read/tracking/unsend / Thrift 漏斗：広告・既読・テレメトリ・消去防止
│   ├── AdViewBlocker        # 广告视图折叠 + addView 扫描再隐藏 / ad-view collapse + addView rescan / 広告ビュー折りたたみ＋addView 再スキャン
│   ├── HomeTabHider         # 底栏标签隐藏（含 tab-inventory 校准日志）/ bottom-tab hiding (+tab-inventory log) / 下部タブ非表示（tab-inventory ログ付き）
│   ├── HomeTopBar           # 主页整页折叠 + 原生顶栏 / home collapse + native top bar / ホーム折りたたみ＋ネイティブ上バー
│   ├── CallTone / SyncGuard / DarkModeHooks / HostCtx
└── ui/SettingsActivity.java # 纯代码三语设置页 / code-only trilingual settings / コードのみの三言語設定画面
tools/                       # 构建流水线 / build pipeline / ビルドパイプライン
```

## 免责声明 / Disclaimer / 免責事項

* 仅供学习与研究 Android Hook 技术使用；修改 LINE 客户端行为可能违反其服务条款。
  For learning & research only; modifying LINE may violate its Terms of Service.
  学習・研究目的のみ。LINE クライアントの改変は利用規約に違反する可能性があります。
* 与 LY Corporation 无任何关联；LINE 及相关商标归原权利人所有。
  Not affiliated with LY Corporation; LINE and related marks belong to their owners.
  LY Corporation とは一切関係ありません。LINE および関連商標は各権利者に帰属します。
* 使用本模块产生的任何后果由使用者自行承担。
  Use at your own risk. 利用は自己責任です。

## 鸣谢 / Credits / 謝辞

本模块为独立实现：下列项目提供功能灵感、技术事实与实现思路参考，代码为重新实现
（无逐行复制），衍生关系详见 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。
This module is an independent implementation: the projects below inspired features,
technical facts and approaches; the code is re-implemented (not line-copied). See
[THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) for derivation details.
本モジュールは独立した実装です。以下のプロジェクトから機能の着想・技術的事実・実装の
考え方を参考にしましたが、コードは再実装したものです（逐行コピーではありません）。
派生関係の詳細は [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) を参照してください。

| 来源 Source 出典 | 贡献 Contribution 貢献内容 | 链接 Link リンク |
| --- | --- | --- |
| **LIME** | 功能点子与风险分级思路（防已读回执/遥测抑制等）、来电铃声/广告折叠手法 / feature ideas & risk classification (anti-read, telemetry suppression...), ringtone & ad-collapse techniques / 機能アイデアとリスク区分（既読対策・テレメトリ抑制など）、着信音・広告折りたたみのテクニック | https://github.com/Chipppppppppp/LIME |
| **LineXtra** (yagiyuu) | 底栏标签隐藏思路与资源条目名（bnb_*）/ bottom-tab hiding approach & resource names (bnb_*) / 下部タブ非表示の考え方とリソース名（bnb_*） | https://github.com/yagiyuu/LineXtra |
| **HonorMarketTamer** | 无 SDK 构建管线与配置三副本方案 / no-SDK build pipeline & 3-copy config scheme / SDK なしビルドパイプラインと設定3重化方式 | https://github.com/mengwuzhuanshou/HonorMarketTamer |
