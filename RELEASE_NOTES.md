Release v1.4.2（versionCode 16）· 校准目标 LINE 26.11.0

⚠ AI-generated module / 本模块由 AI 生成，代码未经人工长期审计，请自行评估风险。
⚠ 遮罩只是视觉遮挡：被遮挡的主页内容仍在后台正常加载、照常消耗流量。

## 更新内容 / What's new

- **配置读取优先级修正**：设置页随开关实时重写的本地两份配置
  （模块 files 目录、shared_prefs 同级副本）现在优先于 /data/local/tmp 副本；
  tmp 仅作末位兜底。曾给过 root 的设备在撤销 root 后继续拨开关，
  不再会被那份永不刷新的陈旧 tmp 文件劫持。
  Config read-order fix: app-owned copies written on every toggle now outrank
  the /data/local/tmp fallback, so devices that lost root keep following the
  latest switches instead of a stale never-refreshed copy.
- 其余功能与 v1.4.1 完全一致：主页深浅色遮罩（长按呼出设置）、底栏
  News/Wallet/Shopping/Mini-Apps 开关、广告请求源头拦截、广告视图折叠、
  来电本地补响、防撤回（实验）、永不回执已读（实验）、子设备深色主题解锁（实验）
- 设置页中/日/英三语；风险分级 [A]视图层 [B]协议层 [C]实验性 在每个开关注明

## 环境要求 / Requirements

- 已 root + Zygisk + LSPosed / rooted device with Zygisk + LSPosed
- LINE 26.11.0（其他版本 hook 可能漂移）/ calibrated against LINE 26.11.0

安装后请在 LSPosed 勾选作用域「LINE」并强制停止 LINE 后重开。
After install, select LINE in the module scope list, then force-stop and reopen it.
