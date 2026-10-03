# ReynardDefaultBrowser

**简体中文** | [English](./README_EN.md)

一个越狱插件：把 [Reynard](https://github.com/minh-ton/reynard-browser)（Gecko 内核的 iOS 浏览器）变成你的**默认浏览器**——本该打开 Safari 的链接会被拦截并转交给 Reynard，设置里也有开关可随时启停。

本仓库是 [guacforlife/ReynardDefault](https://github.com/guacforlife/ReynardDefault) 的 fork，**修复了上游在 iPadOS 15.x 上"设置里点进去就闪退、无法配置"的问题**（修复已提交上游：[PR #10](https://github.com/guacforlife/ReynardDefault/pull/10)）。

## 功能

- **Safari 重定向** — hook SpringBoard 的 `FBSystemServiceOpenApplicationRequest`，把 Safari（及 Firefox / Chrome / Brave）的打开请求换成 Reynard
- **设置开关** — PreferenceLoader 内联开关，直接显示在设置主列表。没有 preference bundle，点开不加载任何代码，从物理上消除了闪退路径
- **控制中心开关** — 通过 CCSupport 快捷启停（可选）

## 适配情况

- 部署目标 **iOS 14.0+**；rootless / rootful 双变体；arm64 / arm64e
- 被 hook 的 API 存在于 iOS 13–16；内联开关在所有支持 PreferenceLoader 的系统上都能正常渲染——包括让上游 bundle 闪退的 iPadOS 分屏设置
- **实测通过：iPad Pro (2021) · iPadOS 15.1 · rootless** ✅（上游 1.5.0 在此环境点击即崩）

| 设备 | 系统 | 越狱 | 变体 | 状态 |
|------|------|------|------|------|
| iPad Pro (2021) | iPadOS 15.1 | rootless | rootless (`iphoneos-arm64`) | ✅ 正常（上游在该环境闪退） |
| iPhone 14 Pro Max | 16.3.1 | Dopamine (Roothide) | rootless (`iphoneos-arm64`) | 正常（上游实测） |
| iPhone 7 | 15.7.7 | Dopamine | rootless (`iphoneos-arm64`) | 正常（上游实测） |
| iPhone 7 Plus | 14.8.1 | Taurine 1.1.7-3 | rootful (`iphoneos-arm`) | 正常（上游实测） |

> 💡 **装之前先看这里**：Reynard 的越狱版 / TrollStore 版 ipa 已内置 `com.apple.developer.web-browser` 权限，部分系统版本的 **设置 → Safari → 默认浏览器 App** 会原生列出 Reynard——如果列表里有，直接选即可，无需本插件。

## 安装

从 [Releases](https://github.com/TsangAsuna/ReynardDefaultBrowser/releases) 下载对应你越狱方案的 deb：

- `*_iphoneos-arm64_rootless.deb` — rootless（Dopamine / Roothide / palera1n rootless）
- `*_iphoneos-arm_rootful.deb` — rootful（unc0ver / palera1n rootful / Taurine）

与上游同包名（`com.guacforlife.reynarddefault`）且版本更高，可**直接覆盖升级**，原有开关状态保留。安装后在设置主列表找到 "ReynardDefault" 开关打开即可。

## 编译

需要 [Theos](https://theos.dev/)：

```bash
export THEOS=~/theos
make rootless   # Dopamine / Roothide
make rootful    # unc0ver / palera1n rootful
make both       # 两种一起构建
```

GitHub Actions 会在每次 push 时自动构建两种 deb，打 tag 时自动发布 Release。

## 许可证

GPL-3.0，详见 [LICENSE](LICENSE)。原始 tweak 由 [guacforlife](https://github.com/guacforlife) 开发。
