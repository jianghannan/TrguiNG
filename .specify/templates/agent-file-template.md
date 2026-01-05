# TrguiNG 开发指南 / TrguiNG Development Guidelines

自动从特性计划生成。最后更新 / Last updated: 2026-01-05

## 核心原则速查 / Core Principle Quickcheck
- 双模式架构：原生 (Tauri + React) 与 Transmission Web 界面功能需对等
- 前端栈：TypeScript + React + Mantine；禁止 jQuery/内联样式
- 后端栈：Rust 1.77+ + Tauri 2.x；Transmission RPC ≥ v16；GeoIP 使用 MMDB
- 性能：Torrent 创建多线程哈希；文件树虚拟滚动；避免阻塞 UI；高效轮询
- 兼容性：Windows/Linux/macOS 三平台；深浅色主题均需验证
- 交付：按用户故事优先级增量迭代；Web 资源自包含（禁 CDN）
- 版本：遵循语义化版本，变更需说明影响面

## 活跃技术 / Active Technologies
- TypeScript (React 18.x, Mantine UI)
- Webpack 构建配置 (`webpack.*.mjs`)
- Rust 1.77+ with Tauri 2.x
- Transmission RPC v16+
- ESLint + TypeScript 严格模式
- MaxMind MMDB (GeoIP)

## 项目结构 / Project Structure

```text
src/                          # 前端源码 / Frontend source
├── index.tsx                 # 应用入口 / App entry
├── index.html                # HTML 模板 / HTML template
├── config.ts                 # 配置管理 / Config management
├── clientmanager.ts          # 客户端管理器 / Client manager
├── cachedfiletree.ts         # 文件树缓存 / File tree cache
├── createtorrent.tsx         # Torrent 创建逻辑 / Torrent creation
├── queries.ts                # 数据查询 / Data queries
├── hotkeys.ts                # 快捷键 / Keyboard shortcuts
├── taurishim.ts              # Tauri 兼容层 / Tauri shim
├── flagsshim.ts              # 国旗图标兼容 / Flags shim
├── themehooks.tsx            # 主题钩子 / Theme hooks
├── trutil.ts                 # 工具函数 / Utility functions
├── components/               # React 组件 / React components
│   ├── app.tsx               # 主应用组件 / Main app component
│   ├── webapp.tsx            # Web 模式入口 / Web mode entry
│   ├── server.tsx            # 服务器连接 / Server connection
│   ├── servertabs.tsx        # 多标签管理 / Multi-tab management
│   ├── toolbar.tsx           # 工具栏 / Toolbar
│   ├── statusbar.tsx         # 状态栏 / Status bar
│   ├── filters.tsx           # 过滤器 / Filters
│   ├── details.tsx           # 详情面板 / Details panel
│   ├── splitlayout.tsx       # 分栏布局 / Split layout
│   ├── contextmenu.tsx       # 右键菜单 / Context menu
│   ├── progressbar.tsx       # 进度条 / Progress bar
│   ├── piecescanvas.tsx      # 分片可视化 / Pieces canvas
│   ├── mantinetheme.tsx      # Mantine 主题配置 / Theme config
│   ├── modals/               # 弹窗组件 / Modal dialogs
│   │   ├── add.tsx           # 添加 Torrent / Add torrent
│   │   ├── settings.tsx      # 设置面板 / Settings panel
│   │   ├── edittorrent.tsx   # 编辑 Torrent / Edit torrent
│   │   ├── edittrackers.tsx  # 编辑 Tracker / Edit trackers
│   │   ├── editlabels.tsx    # 编辑标签 / Edit labels
│   │   ├── move.tsx          # 移动位置 / Move location
│   │   ├── remove.tsx        # 删除确认 / Remove confirm
│   │   ├── daemon.tsx        # 守护进程设置 / Daemon settings
│   │   └── ...               # 其他弹窗 / Other modals
│   └── tables/               # 表格组件 / Table components
│       ├── torrenttable.tsx  # Torrent 列表 / Torrent table
│       ├── filetreetable.tsx # 文件树表 / File tree table
│       ├── peerstable.tsx    # Peers 表 / Peers table
│       ├── trackertable.tsx  # Tracker 表 / Tracker table
│       └── common.tsx        # 表格通用逻辑 / Common table utils
├── rpc/                      # Transmission RPC 客户端 / RPC client
│   ├── client.ts             # HTTP 客户端 / HTTP client
│   ├── torrent.ts            # Torrent 数据模型 / Torrent model
│   └── transmission.ts       # RPC 协议实现 / RPC protocol
├── css/                      # 样式文件 / Stylesheets
├── svg/                      # SVG 图标 / SVG icons
└── types/                    # TypeScript 类型定义 / Type definitions

src-tauri/                    # Rust 后端 / Rust backend (Tauri)
├── Cargo.toml                # Rust 依赖配置 / Rust dependencies
├── tauri.conf.json           # Tauri 应用配置 / Tauri config
├── src/
│   ├── main.rs               # 应用入口 / App entry
│   ├── commands.rs           # Tauri 命令 / Tauri commands
│   ├── createtorrent.rs      # 多线程 Torrent 创建 / Multi-threaded creation
│   ├── poller.rs             # 轮询逻辑 / Polling logic
│   ├── torrentcache.rs       # Torrent 缓存 / Torrent cache
│   ├── geoip.rs              # GeoIP 查询 / GeoIP lookup
│   ├── ipc.rs                # 进程通信 / IPC
│   ├── tray.rs               # 系统托盘 / System tray
│   ├── sound.rs              # 音效播放 / Sound playback
│   ├── integrations.rs       # 系统集成 / System integrations
│   └── macos.rs              # macOS 特定代码 / macOS specific
├── icons/                    # 应用图标 / App icons
└── sound/                    # 音效资源 / Sound assets

dist/                         # Web 构建产物 / Web build output
```

## 常用命令 / Commands

### 开发命令 / Development

| 命令 / Command | 说明 / Description |
|----------------|-------------------|
| `npm install` | 安装前端依赖 / Install frontend dependencies |
| `npm run webpack-serve` | 启动前端开发服务器（热更新）/ Start frontend dev server (HMR) |
| `npm run tauri-dev` | 启动 Tauri 开发模式（自动重载）/ Start Tauri dev mode (auto-reload) |
| `npm run react-devtools` | 启动 React DevTools / Launch React DevTools |
| `npm run info` | 显示 Tauri 环境信息 / Show Tauri environment info |

### 构建命令 / Build

| 命令 / Command | 说明 / Description |
|----------------|-------------------|
| `npm run build` | 完整生产构建（前端 + 原生应用 + 安装包）/ Full production build |
| `npm run build-bin` | 仅构建二进制（不打包安装程序）/ Build binary only (no installer) |
| `npm run webpack-dev` | 开发模式 Webpack 构建（未压缩）/ Webpack dev build (unminified) |
| `npm run webpack-prod` | 生产模式 Webpack 构建（压缩优化）/ Webpack prod build (minified) |

### Rust/Cargo 命令 / Rust/Cargo

| 命令 / Command | 说明 / Description |
|----------------|-------------------|
| `cd src-tauri && cargo build` | 编译 Rust 后端（调试模式）/ Build Rust backend (debug) |
| `cd src-tauri && cargo build --release` | 编译 Rust 后端（发布模式）/ Build Rust backend (release) |
| `cd src-tauri && cargo check` | 快速语法检查（不生成二进制）/ Quick syntax check (no binary) |
| `cd src-tauri && cargo clippy` | Rust 代码静态分析 / Rust linting |
| `cd src-tauri && cargo fmt` | 格式化 Rust 代码 / Format Rust code |

### 代码检查 / Linting

| 命令 / Command | 说明 / Description |
|----------------|-------------------|
| `npx eslint src/` | ESLint 检查前端代码 / Lint frontend code |
| `npx eslint src/ --fix` | ESLint 自动修复 / Auto-fix lint issues |
| `npx tsc --noEmit` | TypeScript 类型检查（不输出文件）/ TypeScript type check |

### GeoIP 数据库 / GeoIP Database

| 命令 / Command | 说明 / Description |
|----------------|-------------------|
| `wget -nv -O src-tauri/dbip.mmdb "https://github.com/openscopeproject/TrguiNG/releases/latest/download/dbip.mmdb"` | 下载 GeoIP 数据库 / Download GeoIP database |

### 开发工作流示例 / Dev Workflow Example

```bash
# 1. 安装依赖 / Install dependencies
npm install

# 2. 下载 GeoIP 数据库（首次）/ Download GeoIP DB (first time)
wget -nv -O src-tauri/dbip.mmdb "https://github.com/openscopeproject/TrguiNG/releases/latest/download/dbip.mmdb"

# 3. 并行启动前端和 Tauri（两个终端）/ Start frontend + Tauri (two terminals)
# 终端 1 / Terminal 1:
npm run webpack-serve
# 终端 2 / Terminal 2:
npm run tauri-dev

# 4. 提交前检查 / Pre-commit checks
npx eslint src/
npx tsc --noEmit
cd src-tauri && cargo clippy
```

## 代码风格 / Code Style
- 遵循 `eslint.config.mjs`，提交前必须无 lint 违规
- TypeScript 严格模式，避免 `any`，若使用需说明理由
- React 函数组件 + Hooks；避免类组件与未使用的状态
- 样式使用 Mantine 主题或 CSS 模块；禁止内联样式、禁止 jQuery
- 不硬编码服务器地址；所有端点需可配置
- 日志与错误信息保持可调试且避免泄露敏感数据

## 测试与质量门禁 / Testing & Quality Gates
- TypeScript 编译通过；ESLint 通过
- 原生模式测试（Windows/Linux/macOS）；Web 模式测试（Transmission web）
- 深色/浅色主题渲染正确
- 核心性能场景验证：Torrent 创建、文件树渲染、实时刷新
- 按语义化规则更新版本；必要时同步 `package.json` 与 `Cargo.toml`

## 最近变更 / Recent Changes

### v1.5.1 (当前版本 / Current)
- **文件重命名优化**：选中文件名主体（不含扩展名）；修复重命名时文本被意外重选的问题
- **代理透传修复**：允许代理 fetch 时传递重复 headers
- **Torrent 位置逻辑**：修复预定义目录检查的禁用逻辑

### v1.5.0
- **窗口关闭行为修复**：修复使用关闭模式（而非退出/隐藏）时窗口无法关闭
- **添加 Torrent 优先级**：修复添加 torrent 时优先级选择被忽略的问题
- **创建 Torrent 主题**：修复创建 torrent 窗口主题不匹配的问题
- **Transmission 兼容性**：修复与 Transmission 3.0 以下版本的兼容问题
- **macOS 退出菜单**：修复从退出菜单无法退出应用的问题
- **Linux 托盘图标**：修复生成的托盘图标路径使其唯一
- **设置对话框**：改进设置对话框的间距和布局

<!-- MANUAL ADDITIONS START -->
<!-- MANUAL ADDITIONS END -->
