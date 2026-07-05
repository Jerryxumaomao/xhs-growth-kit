# 从零安装（含全部已知坑）

## 通用前置

| 依赖 | 用途 | 装法 |
|---|---|---|
| Claude Code | 一切的宿主 | https://claude.com/claude-code |
| Python 3.10+ | 适配器/剪辑 helpers | 官网或 winget/brew |
| git | 台账即档案（init 后请立刻 `git init` 你的项目！） | — |
| jq | SessionStart 状态看板 | Win: 下载 jq.exe 放进 Git Bash 的 /usr/bin；mac: `brew install jq` |
| Playwright + Chromium | xhs-explore 抓创作者后台 | `pip install playwright && playwright install chromium` |
| ffmpeg（剪视频才需要） | video-use | Win: `winget install Gyan.FFmpeg`；mac: `brew install ffmpeg` |

## 初始化流程

1. clone 本仓库为你的项目目录（或把 `cheat-on-content/` 拷进你的项目）
2. Claude Code 里说「初始化 cheat-on-content」，跟着走：
   - 内容形态：**涨粉选 i / 种草选 h**（目标不同 rubric 不同，想清楚再选）
   - 强烈建议导入 5-10 个对标账号（cheat-learn-from）
   - hooks 装上（预测锁 + 开屏状态报告）
3. xhs-explore 登录：`python cheat-on-content/adapters/perf-data/xhs-explore/crawler.py login` 扫码创作者中心，cookie 存 `.auth-xhs/`（已在 .gitignore）
4. **立刻 `git init` + 首次提交**——预测不可变、rubric 演化、复盘档案全都依赖 git 兜底

## Windows 专属坑（全部真实踩过）

| 坑 | 症状 | 解 |
|---|---|---|
| 控制台 GBK 编码 | Python 打 emoji 直接崩 `UnicodeEncodeError` | 用户级环境变量 `PYTHONUTF8=1`，新开终端生效 |
| 符号链接建不了 | `New-Item SymbolicLink` 要提权 | 用 **Junction**（`New-Item -ItemType Junction`）注册全局 skill，不用提权 |
| winget 装完 ffmpeg 找不到 | PATH 注册表更新了但当前会话没刷新 | 新开终端，或把 ffmpeg bin 写进 `~/.bashrc` |
| npm 直连超时 | `ECONNRESET` | `--registry=https://registry.npmmirror.com` |
| hook 不触发 | jq 不在 PATH | jq.exe 放 Git Bash /usr/bin |

## 抓取相关

- 创作者后台接口偶发 `ERR_CONNECTION_CLOSED`（代理/风控）→ 重试即可
- 涨粉数据只在「数据看板→内容分析」接口有，笔记列表接口没有——适配器已处理，别自己改路由
- 选题研究另装 OpenCLI（`npm i -g @jackwener/opencli`）：`opencli xiaohongshu search "关键词"` 复用 Chrome 登录态（Chrome 需开着且已登录小红书）
