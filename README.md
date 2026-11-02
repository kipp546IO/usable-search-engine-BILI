# 能用的搜索引擎（usable-search-engine-BILI）哔哩哔哩版

> 本仓库是 **哔哩哔哩版**：只保留 B 站搜索（综合 / 名称 / 简介 / 标签 / UP主 / UID），不含网易云部分。

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![dependencies](https://img.shields.io/badge/dependencies-0-brightgreen)
![license](https://img.shields.io/badge/license-MIT-blue)
![platform](https://img.shields.io/badge/platform-Windows%20%7C%20macOS%20%7C%20Linux-lightgrey)
![version](https://img.shields.io/badge/version-v0.1.0-orange)

中文 · [English](README.en.md)

<h3 align="center">
<a href="https://github.com/kipp546IO/usable-search-engine-BILI/releases/download/v0.1.0/usable-search-bilibili-v0.1.0.zip">⬇️ 点这里下载 ZIP · 一键启动</a>
&nbsp;&nbsp;&nbsp;&nbsp;
<a href="https://kipp546IO.github.io/usable-search-engine-BILI/share/">↗ 分享功能</a>
</h3>

<p align="center"><sub>下载后解压 → 双击 <code>install.cmd</code> → 浏览器自动打开。不需要装别的东西。</sub></p>

<h2 align="center">你还在忍受 B 站的搜索吗？NO——排序不该由推荐算法替你做主</h2>

<p align="center"><img src="docs/banner.jpg" width="720" alt=""></p>

## 🔍 这是什么

一个**自建索引**的 B 站搜索工具：自己收数据 → 建本地索引 → 按你的规则排序展示，不转发平台的推荐排序。

搜索维度六种：综合 / 名称 / 简介 / 标签 / UP主 / UID。

## 💥 为什么做这个

B 站搜索的问题不是"搜不到"，而是**结果页里先塞一堆推荐，真正的结果夹在中间**：

- 打开结果页，前几屏大半是热门、相关推荐和推广，真正命中关键词的只有零星几条，得往下翻才找得到；
- 关键词明明在标题里，播放量高、发布早的原视频却被新发的搬运和切片挤到后面；
- 搜 UP 主名字，先给你一堆名字里恰好带那几个字的无关视频，真正的主页要自己一个个点进去认；
- 简介和标签平台根本不当作检索项，你想找的东西明明写着那几个字，就是搜不出来；
- 排序只有平台给的那几档，"按播放量 + 只看标题含关键词"这种组合不给，或者给了也不准。

实测：搜「影色舞」扫了 100 条，按标题/简介/标签认真匹配后只有 48 条真的命中——**差不多一半是推荐混进来的**。

这个工具就是把结果从推荐堆里捞出来，再按你的规则排一遍。

## 🧩 与现有项目的差异

- 不做浏览器插件、不改页面、不注入脚本；它是一个**独立的本地服务**，只读平台公开接口
- 不代理、不镜像视频内容；只索引元数据（标题、简介、标签、UP主、播放量、时长、封面地址）
- 排序规则写在本地：相关度、命中位置、播放量、发布时间都能自己组合
- 全部数据留在本机，不上传、不登录、不需要账号

## 🛠 技术栈

纯 Python 3.10+（**只用标准库**：urllib / sqlite3 / http.server / json / threading）＋ SQLite＋ 单页 HTML（无框架、无构建）

## ⚡ 三分钟上手（零基础也看得懂）

### 第 1 步 · 装 Python（只装一次）

1. 打开 <https://www.python.org/downloads/>，点黄色大按钮下载
2. 双击安装包，**第一屏底部一定要勾上 `Add python.exe to PATH`**，再点 Install Now
3. 装完把弹出的黑窗口都关掉

> 怎么确认成功：按 `Win + R`，输入 `cmd` 回车，在黑窗口里打 `python -V`，能看到 `3.10` 以上就行

### 第 2 步 · 拿到项目

- **省事版**：本页右上角点 `Code` → `Download ZIP`，解压到任意目录（别放 `C:\Program Files`，那里没写权限）
- **Git 版**：`git clone https://github.com/kipp546IO/usable-search-engine-BILI.git`

### 第 3 步 · 双击启动（就一下）

- **Windows**：双击 `install.cmd`。会弹出一个黑窗口，几秒后浏览器自动打开页面
- **macOS / Linux**：在项目目录里执行 `bash install.sh`

**那个黑窗口要留着** —— 它就是服务本身，关掉窗口 = 关掉服务。想停就关它。

### 第 4 步 · 开始搜

地址固定是 <http://127.0.0.1:18788/>，建议加进收藏夹。输入视频标题、UP 主名字或标签回车即可

- 第一次搜要现场扫多页，慢几秒是正常的；同一查询 10 分钟内走缓存
- 筛选面板可以按时长、发布时间、排序、匹配标准过滤

### 下次再开

再双击一次 `install.cmd` 就行，没有别的步骤

## 📦 下载即用（快速开始）

需要 Python 3.10 或更高版本。**不需要装任何第三方库**

方式一（推荐）：装成包再用，命令最干净

```
git clone https://github.com/kipp546IO/usable-search-engine-BILI.git
cd usable-search-engine-BILI
python -m venv .venv
.venv\Scripts\activate          # Windows；macOS/Linux 用 source .venv/bin/activate
pip install -e .
python -m usable_search serve
```

方式二（完全免安装、不碰 pip）：直接把 `src` 加进模块路径

- Windows PowerShell：`$env:PYTHONPATH="src"; python -m usable_search serve`
- Windows cmd：`set PYTHONPATH=src && python -m usable_search serve`
- macOS/Linux：`PYTHONPATH=src python -m usable_search serve`
- 或者直接双击 `start-search-service.cmd`（仅 Windows，自带守护，崩了 5 秒自动重启）

起来以后浏览器打开 <http://127.0.0.1:18788/>

- 数据目录 `data/` 首次运行自动创建（索引、档案、探针），不进 git
- 服务只监听 `127.0.0.1`，不对外网开放，不会暴露到局域网或公网
- 换端口：`python -m usable_search serve --port 你的端口`

### 使用前请知悉

- 抓取走的是 B 站的**公开接口（非官方）**，风控按 IP 判断：**请低频使用**
- 本项目不提供、不转发、不缓存任何视频内容；只保存元数据，仅供个人研究与检索使用
- 许可：MIT，见 `LICENSE`

## ✨ 功能一览（v0.1.0）

![界面](docs/screenshot.png)

**搜索**

- 六种维度：综合 / 名称 / 简介 / 标签 / UP主 / UID
- 多通道召回：关键词窄的时候补扫最新、播放、弹幕、收藏几个入口，把被算法埋掉的条目捞回来
- 命中位置可见：每张卡片标出「命中：标题 / 简介 / 标签」
- 分页 30 / 50 / 100 / 300，页间不重复

**筛选**

- 时长：1 分钟以内 / 1~5 / 5~10 / 10~30 / 30 分钟以上 / 自定义分钟区间
- 发布时间：今天 / 一周内 / 一个月内 / 一年内 / 一年前 / 自定义日期区间
- 排序：综合 / 综合热度 / 最新 / 最旧 / 播放量 / 弹幕 / 收藏（投币与转发平台不开放，置灰）
- 匹配标准：标准（标题/简介/标签任一）· 标准·不含标签 · 严格（仅标题）· 宽松（平台原样，噪音会混入）

**档案与回看**

- 每次搜索自动把见到的视频记进本地档案
- 一键复查：逐条问平台详情接口，分成「正常 / 已删除 / 仅自己可见 / 查不到」四类；查不到绝不当成已删除
- 播放备份站开关（**默认关闭**）：只发 HEAD 请求、1 秒间隔、串行、命中永久缓存、未命中 7 天后重试

**体验**

- 深色 / 浅色主题，首次跟随系统，之后记住选择
- 结果内二次筛选：不重新联网，只在已拿到的结果里过滤
- 搜索历史存在本机浏览器（可单条删除，不上传）
- 清缓存面板，点之前先说清会清掉什么

**工程**

- 纯 Python 标准库，零第三方依赖
- 本地 SQLite 索引，搜索词不写进探针库
- 接口可用性探针：成功率、p50、p95，风控单独计数

## ❓ 常见问题

**会不会把我账号封了？**
不会。全程匿名、不带登录态、不发账号操作请求，只读公开搜索接口。真正要注意的是**别高频刷**——风控跟 IP 走，刷太猛会被临时挡（表现为返回 -352）

**为什么有的视频搜不到，或者很慢？**
搜索是现场扫页 + 本地索引，第一次查要联网。关键词太窄时平台本身不给结果，我们会补扫几个入口，所以会慢一些；换关键词通常更快

**UP主的全部投稿能列出来吗？**
还不能。投稿列表接口在风控名单里，稳定拿不到，这一项暂时没做（宁可不做，也不拿残缺数据充数）

**「已删除」判断准吗？**
只有平台明确返回"不存在"才记已删除；返回"仅自己可见"记为隐藏；网络错误、频率限制这类一律记"查不到"，不会冤枉视频

**数据存在哪？删了会怎样？**
在项目目录的 `data/` 里。删掉整个 `data/` 等于清空索引和档案，下次搜索会重新积累

**我的搜索记录会被上传吗？**
不会。历史只存在你浏览器的 localStorage；服务端只记接口成功率和耗时，不落搜索词

**端口 18788 被占了怎么办？**
启动时加 `--port 其他端口`，或者改 `install.cmd` 顶部的 `PORT`

**怎么卸载？**
删掉整个项目文件夹即可。它是绿色的，不写注册表、不开机自启

## 🚧 边界

- 数据仅供个人使用；抓取遵守平台限速、礼貌访问；
- 开源仓库不含任何抓取回的内容数据；
- 不提供任何受版权保护内容的下载与再分发；
- 备份站是第三方存档，命中只代表"有存档"，不代表能播放或画质可靠。
