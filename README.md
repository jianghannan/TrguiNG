# TrguiNG

> **声明 / Disclaimer**
>
> 本仓库是 [OpenScopeProject/TrguiNG](https://github.com/OpenScopeProject/TrguiNG) 的非官方分支版本，主要用于添加多语言支持。
>
> This repository is a fork of [OpenScopeProject/TrguiNG](https://github.com/OpenScopeProject/TrguiNG), primarily for adding multi-language support and localization improvements.

## 本分支特点 / Fork Features

相比上游仓库，本分支增加了以下功能：

Compared to upstream, this fork adds:

- 🌐 **完整的国际化支持 / Full i18n Support**
  - 简体中文 (zh-Hans) / Simplified Chinese
  - 繁体中文 (zh-Hant) / Traditional Chinese
  - 英文 (en) / English
- 🔤 **界面完全本地化 / Fully Localized UI**
  - 所有菜单、按钮、状态信息均已翻译 / All menus, buttons, and status messages translated
  - 节点状态、Tracker 状态等动态内容支持翻译 / Dynamic content like peer status, tracker status translated
  - 国家名称根据系统语言自动显示 / Country names auto-localized via system language
- 📚 **双语文档 / Bilingual Documentation**
  - 规范文档中英双语 / Spec documents in Chinese and English
  - 术语表保证翻译一致性 / Glossaries ensure translation consistency
- 🛠️ **Speckit 规范驱动开发 / Speckit Spec-Driven Development**
  - 项目宪章定义核心原则 / Project constitution defines core principles
  - 功能规范模板确保一致性 / Feature spec templates ensure consistency
  - 支持 `/speckit.spec`、`/speckit.plan`、`/speckit.tasks` 工作流 / Supports speckit workflow commands

## 国际化开发指南 / i18n Development Guide

本项目使用 [i18next](https://www.i18next.com/) 进行国际化。/ This project uses i18next for internationalization.

### 目录结构 / Directory Structure

```
src/i18n/
├── index.ts              # i18n 初始化 / initialization
├── languages.ts          # 语言资源 / language resources
├── locales/
│   ├── en.json           # English
│   ├── zh-Hans.json      # 简体中文
│   └── zh-Hant.json      # 繁体中文
├── glossary.zh-Hans.md   # 简体术语表 / Simplified glossary
└── glossary.zh-Hant.md   # 繁体术语表 / Traditional glossary
```

### 使用方法 / Usage

```tsx
import { useTranslation } from "i18n";

function MyComponent() {
  const { t } = useTranslation();
  return <div>{t("section.key")}</div>;
}
```

### 添加新语言 / Adding New Language

1. 创建 `src/i18n/locales/{lang}.json` / Create locale file
2. 在 `languages.ts` 中注册 / Register in languages.ts
3. 翻译所有键值 / Translate all keys

### 翻译覆盖率检查脚本使用 / Using Translation Coverage Check Script

```bash
node scripts/check-i18n-coverage.js
```

## 以下内容为原始上游仓库 README.md 文档 / The following is the original README.md from upstream

---

**Remote GUI for Transmission torrent daemon**

![GitHub release](https://img.shields.io/github/v/release/OpenScopeProject/TrguiNG)
![Downloads](https://img.shields.io/github/downloads/OpenScopeProject/TrguiNG/total)
![Lint status](https://img.shields.io/github/actions/workflow/status/OpenScopeProject/TrguiNG/lint.yml?label=Lint&event=push)

![logo](https://i.imgur.com/QdgMWwW.png)

`TrguiNG` is a rewrite of [transgui](https://github.com/transmission-remote-gui/transgui)
project using [tauri](https://tauri.app).
Frontend is written in typescript with [react.js](https://react.dev/) and
[mantine](https://mantine.dev/) library. Backend for the app is written in
[rust](https://www.rust-lang.org/).

You can use this program in 2 ways: as a native Windows/Linux/Mac app and as a web gui
served by transmission itself by setting `$TRANSMISSION_WEB_HOME` environment variable
to point to TrguiNG web assets.

There are screenshots of the app available on the
[project wiki](https://github.com/openscopeproject/TrguiNG/wiki).

Some differentiating features:

* Multi tabbed interface for concurrent server connections (native app only)
* Torrent creation with fast multi threaded hashing (native app only)
* Powerful torrent filtering options
* Latest transmission features support: labels, bandwidth groups, sequential download
* Dark and white theme

Planned:

* Better bandwidth groups support when API is ready (https://github.com/transmission/transmission/issues/5455)

Transmission v2.40 or later is required.

## Downloads

You can get the latest release from the
[releases page](https://github.com/openscopeproject/TrguiNG/releases).

Weekly builds of current development version are available from github
[build workflows](https://github.com/openscopeproject/TrguiNG/actions/workflows/build.yml).
Pick the latest successful run and scroll down to the artifacts section.

## Compiling

Prerequisites:

- [Node.js 16](https://nodejs.org/) or later
- [rust 1.77](https://www.rust-lang.org/) or later
- Geoip lookup database in mmdb format, put it in `src-tauri`

  ```
  wget -nv -O src-tauri/dbip.mmdb "https://github.com/openscopeproject/TrguiNG/releases/latest/download/dbip.mmdb"
  ```

  You can get latest db from [db-ip.com](https://db-ip.com/db/download/ip-to-country-lite).

To compile simply run

```
$ npm install
$ npm run build
```

This will generate optimized bundle in `dist` and a release binary in `src-tauri/target/release` folder.
Also installer package will be available in `src-tauri/target/release/bundle/...`.

The binary is statically linked and embeds all necessary assets except for the geoip database.
It is completely self sufficient and can be used as a portable executable but for geoip lookup to work you
need to install the app with provided installer.

For development run in parallel

```
$ npm run webpack-serve
$ npm run tauri-dev
```

Webpack will automatically watch changes in `src/` and refresh the app view, tauri will watch changes
in `src-tauri/` and rebuild/restart the app as needed.

## How to use TrguiNG as a web interface

Transmission supports custom web interfaces, all you have to do is run the daemon with
`$TRANSMISSION_WEB_HOME` variable pointing to the web assets that transmissinon will serve
over it's `.../transmission/web/` endpoint.

Example steps for debian:

1. Download latest `trguing-web-xxxx.zip` zip from [releases](https://github.com/openscopeproject/TrguiNG/releases)
   page.
2. Unpack it anywhere, make sure that the user transmission runs under (by default `debian-transmission`)
   has read permissions.
3. Edit transmission daemon systemd unit file `/etc/systemd/system/multi-user.target.wants/transmission-daemon.service`
   and add following to `[Service]` section:
   ```
   Environment=TRANSMISSION_WEB_HOME=/path/to/extracted/trguing/zip
   ```
4. Reload the unit file with `sudo systemctl daemon-reload`
   and restart the service `sudo systemctl restart transmission-daemon`

## License

Project is distributed under GNU Affero General Public License v3, see `LICENSE.txt` for details.
