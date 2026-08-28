# LineTamer v1.6.0（versionCode 18）

校准目标：LINE 26.11.0，已在 26.13.0 实机全量验证 / Calibrated against LINE 26.11.0, fully verified on 26.13.0 / LINE 26.11.0 基準、26.13.0 で実機検証済み

⚠ AI-generated module / 本模块由 AI 生成，代码未经人工长期审计，请自行评估风险。

---

## 中文

**主页净化重构：底栏版**

- 底栏只留**聊天**一个 tab：主页等其余 tab 全部隐藏，聊天自动左对齐。
- **冷启动直落聊天页**：对聊天 tab 注入一次合成触摸（由 LINE 自身控制器解析页面位置，免疫主页 pager 页序漂移），并带 DB 门控——LINE 自己恢复到聊天页时不注入，杜绝聊天/好友来回翻转。
- 原**主页顶栏功能**下放到底栏右侧的**功能区**：
  - 头像 + 账户昵称（通讯录库只读查询，可一键隐藏昵称防肩窥）→ 打开自己的个人资料
  - 服务 / 通知 / 设置（显式 Intent 直开 LINE 对应页面；大屏设备走双栏设置页，子选项正常进入）
- **配色按底栏实际明暗自动适配**（读底栏背景图像素，无监听、零常驻）。LINE 的应用内主题独立于系统深浅色设置，故另备**手动浅色配色**开关。
- 功能区**垂直居中对齐可见底栏**（运行时测量锚点，适配任意分辨率/屏占比/手势条模式）。
- 移除旧遮罩/折叠方案及其失效开关；设置页只留有效选项，中/日/英三语并按 [A]视图层 / [B]协议层 / [C]实验性 标注风险。
- 主页内容网络拦截保持默认开启（getHomeServiceList / getInstantNews 请求直接不发，主页不可达后自然零流量）。
- 其余功能不变：广告请求源头拦截、广告视图折叠、来电本地补响、防撤回（实验）、永不回执已读（实验）、子设备深色主题解锁（实验）。
- AI 入口不做：其弹层需要服务端下发参数，静态启动会崩宿主。

## 日本語

**ホーム浄化の作り直し：ボトムバーエディション**

- 下バーには**トーク**タブだけを残し、ホーム等の他タブはすべて非表示（トークは左寄せ）。
- **コールドスタートは直接トークページへ**：トークタブに合成タッチを一度注入（LINE 自身のコントローラーがページ位置を解決し、ページ順の変動に強い）。DB ガード付きで、LINE 自身がトークを復元した場合は注入しないのでトーク/友達の往復切替は起きません。
- 旧ホーム上バーの機能は下バー右側の**機能エリア**へ：
  - アバター＋アカウント名（連絡先 DB の読み取り専用照会、ワンタップで非表示化）→ 自分のプロフィール
  - サービス / 通知 / 設定（明示的 Intent で直接起動。大画面はツーペインの設定画面を開くのでサブ項目も正常）
- **配色は下バーの実際の明暗に自動追従**（下バー背景ビットマップから読み取り、リスナー常駐なし）。LINE のテーマは端末のダーク/ライト設定と独立しているため、**手動ライト配色**スイッチも用意。
- 機能エリアは**可視バーに対して垂直中央揃え**（実行時測定アンカー、解像度/画面比率/ジェスチャーバー不問）。
- 旧マスク/折りたたみ方式と無効化されたスイッチを削除。設定画面は中日英 3 言語、[A]表示層 / [B]通信層 / [C]実験的 のリスク表記付き。
- ホームフィードの通信遮断はデフォルト ON（getHomeServiceList / getInstantNews を送信せず、ホーム到達不可で自然にゼロトラフィック）。
- 他機能は変更なし：広告リクエスト遮断、広告ビュー折りたたみ、着信ローカル着信音、防止送信（実験）、既読送信なし（実験）、サブ端末ダークテーマ解除（実験）。
- AI 入口は非対応：シートにサーバー発行パラメータが必要で、静的起動するとホストがクラッシュします。

## English

**Home purification rebuilt: the bottom-bar edition**

- The bottom bar keeps only the **chat** tab: the home tab and every other tab are hidden, chat left-aligned.
- **Cold starts land directly on the chat page**: a single synthetic tap is injected on the chat tab (LINE's own controller resolves the page position — immune to home pager page-order drift), gated by a DB check so nothing is injected when LINE restores chat by itself; no chat/friends flip-flopping.
- The former **home top-bar functions** move into a **function area** on the right of the bottom bar:
  - Avatar + account nickname (read-only contacts DB query; one-tap hide for shoulder-surfing privacy) → opens your own profile
  - Services / Notifications / Settings (explicit intents straight to LINE's pages; large screens get the two-pane settings activity, sub-pages open fine)
- **The area's colors auto-match the bar's actual light/dark appearance** (read from the bar background bitmap; no listeners, nothing resident). LINE's in-app theme is independent of the system dark mode, so a **manual light palette** switch is also provided.
- The area is **vertically centered on the visible bar** (runtime-measured anchors; any resolution / aspect ratio / gesture-bar mode).
- The legacy mask/collapse scheme and its dead switches are removed; the settings page lists only effective options, in Chinese/Japanese/English with risk tags [A] view-tier / [B] protocol-tier / [C] experimental.
- Home-feed network blocking stays on by default (getHomeServiceList / getInstantNews are never sent — naturally zero traffic once home is unreachable).
- Everything else unchanged: source-level ad blocking, ad-view collapse, local ringtone on incoming calls, anti-unsend (experimental), never-send-read-receipts (experimental), dark-theme unlock on sub devices (experimental).
- No AI entry: its sheet needs server-issued parameters and crashes the host when started statically.

---

## 环境要求 / Requirements / 環境要件

- 已 root + Zygisk + LSPosed / rooted device with Zygisk + LSPosed / root 済み + Zygisk + LSPosed
- LINE 26.11.0 / 26.13.0（其他版本 hook 可能漂移 / other versions may drift / 他バージョンはずれる可能性）

安装后请在 LSPosed 勾选作用域「LINE」并强制停止 LINE 后重开。
After install, select LINE in the module scope list, then force-stop and reopen it.
インストール後、LSPosed でスコープ「LINE」を有効化し、LINE を強制停止してから再起動してください。
