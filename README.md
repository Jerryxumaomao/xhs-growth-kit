# 小红书起号工具箱 · XHS Growth Kit

> 用 Claude Code 把小红书起号变成一条**可校准的流水线**：选题 → 写稿 → 盲评打分 → 拍摄/剪辑 → 发布 → 数据复盘 → 进化打分规则。
> 不承诺爆款。承诺的是：**每一篇都变成数据，数据逼着下一篇更好。**

这套东西在一个真实的从零账号上跑通过：3 篇内容、全流程闭环、每一处工具坑都踩过并修掉了。仓库里没有任何账号定位/人设模板——**定位是你自己的事**，工具箱负责把你的定位跑成系统。

---

## 里面有什么

| 目录 | 是什么 |
|---|---|
| `cheat-on-content/` | 核心引擎（MIT，含本仓库的小红书扩展与大量修复，见下）——打分 rubric、盲评隔离、发布/复盘状态机、数据抓取适配器 |
| `video-use-patches/` | [browser-use/video-use](https://github.com/browser-use/video-use)（对话式剪视频）的 4 个补丁：Windows 字幕路径 / 中文整句字幕+样式配置 / overlay 缩放 / 音频降噪链 |
| `docs/SETUP.md` | 从零安装（Windows/macOS），含全部已知坑 |
| `docs/PIPELINE.md` | 五阶段流水线怎么跑 + 外部 skill 接线表 |

### 相比上游 cheat-on-content 多了什么（本仓库的增量）

- **小红书双 rubric**：`starter-rubrics/xhs-zhangfen-zero.md`（涨粉/口碑，北极星=关注转化率）+ `xhs-zhongcao-zero.md`（种草，北极星=收藏率）——init 时按你的阶段目标二选一，**涨粉和种草是会打架的两个目标，别都要**
- **xhs-explore 适配器**：Playwright 拦截创作者后台真实接口，复盘自动抓 曝光/阅读/**涨粉**/封面点击率/评论（含分页、公开页兜底）
- **修复**：`pending_retros` 契约统一（对象形态+已复盘标记）、SessionStart hook 的 T+N 计算、盲评泄漏防护的自相矛盾（观察模板 vs grep 自检）、涨粉字段候选统一、内容分析分页

---

## 快速开始

```bash
# 0. 前置：Claude Code + Python 3.10+ + git；Windows 另需 jq（放进 Git Bash /usr/bin）
git clone https://github.com/<you>/xhs-growth-kit my-xhs-project
cd my-xhs-project

# 1. 在 Claude Code 里说：
初始化 cheat-on-content
# 跟着问答走：内容形态选 i) 小红书涨粉 或 h) 小红书种草；导入 5-10 个对标账号

# 2. 之后的日常就是五句话：
找选题        # 候选池 brainstorm / 抓热点
启动预测 scripts/xxx.md   # 盲评打分并锁定预测（发布前必须做）
拍了          # 拍摄登记，buffer +1
已发布        # 发布登记，进入复盘队列
复盘 xxx      # T+3 天后，自动抓数据对照预测
```

## 三条不可妥协的纪律（工具的灵魂，不遵守=白装）

1. **盲评**：打分由隔离的 sub-agent 完成，它只能看稿子和规则，看不到任何历史数据。预测一旦写下就锁死（hook 强制），复盘只能追加。
2. **复盘必须做**：T+3 天抓真实数据对照预测。rubric 的每一次进化都只能来自"预测 vs 实绩"的偏差。
3. **别跳步骤**：跳过盲评直接发 = 这篇对系统没有贡献；改预测让自己"看起来准" = 整个系统作废。

## 已被真实数据验证过的两条经验（省你两篇学费）

- **互动 ≠ 涨粉**。点赞评论再热闹，不给陌生人一个"关注你这个人"的理由（连载/人设/方法论），涨粉就是 0。别拿赞评安慰自己。
- **封面点击率是分发的咽喉**。内容再好，CTR 不行算法就不给池子。封面露人、有记忆点，比正文多打磨一小时更值。

## 视频剪辑（可选层）

装 [video-use](https://github.com/browser-use/video-use) 后打上 `video-use-patches/`：

```bash
git clone https://github.com/browser-use/video-use ~/Developer/video-use
cd ~/Developer/video-use && git am /path/to/xhs-growth-kit/video-use-patches/*.patch
```

之后：手机拍原始素材（每句拍 2-3 条，说错不用停）丢进文件夹 → 对 Claude 说"剪成 45s 竖屏成片，中文字幕" → 出成片。中文整句字幕、去口癖、降噪、动效插槽都能用。经验参数见 `docs/PIPELINE.md`。

## License

- `cheat-on-content/` 保持上游 MIT（见其 LICENSE）
- 本仓库其余内容 MIT

祝起号顺利。记住：**这套系统不保证你第一篇就成，它保证你第五篇比第一篇聪明。**
