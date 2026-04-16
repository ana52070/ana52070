# 部署说明 · Setup Guide

## 📁 最终目录结构

你需要在 `ana52070/ana52070` 这个仓库里放这几个文件：

```
ana52070/                          ← GitHub 上的仓库名必须和你用户名一致
├── README.md                      ← 主文件，直接替换
└── .github/
    └── workflows/
        ├── snake.yml              ← 贡献图蛇形动画生成器
        └── blog-post-workflow.yml ← 博客文章自动同步
```

---

## 🚀 三步上线

### Step 1：创建 Profile 仓库

1. 到 GitHub 新建仓库，**仓库名必须是 `ana52070`**（和你的用户名完全一致）
2. 勾选 "Add a README file"、设为 Public
3. 把我给你的 `README.md` 内容粘贴进去，提交

完成这一步你的主页就已经能看到动态卡片了（Stats、Streak、Top Languages、Trophy 这些开箱即用，零配置）。

### Step 2：开启蛇形贡献图（可选但强烈推荐）

1. 在仓库里创建目录 `.github/workflows/`
2. 把 `snake.yml` 上传进去
3. 到仓库 **Settings → Actions → General**，把 "Workflow permissions" 改成 **"Read and write permissions"**，保存
4. 到 **Actions** 标签页，手动点一次 "Generate Snake Animation" 的 Run workflow
5. 等一分钟，它会自动创建一个 `output` 分支存放生成的 SVG 图

之后每天 UTC 00:00（北京 08:00）自动跑一次。

### Step 3：开启 Wiki 博客自动同步（可选）

前提：你的 `chuiyu.wiki` 必须有 **RSS feed**。

先确认你的 RSS 地址。在浏览器打开下面任一 URL 测试：
- `https://chuiyu.wiki/feed.xml` ← VitePress/VuePress 默认
- `https://chuiyu.wiki/rss.xml`
- `https://chuiyu.wiki/atom.xml`
- `https://chuiyu.wiki/feed`

**如果都返回 404**，说明你的 Wiki 没有配 RSS，需要先去给它加一个。大部分静态站点生成器（Hugo/Hexo/VitePress/Astro）都有对应插件，一行配置的事。

确认 RSS 地址可用后：
1. 编辑 `blog-post-workflow.yml` 里的 `feed_list` 字段，改成你实际的 RSS URL
2. 上传到 `.github/workflows/blog-post-workflow.yml`
3. 手动触发一次测试

如果暂时不想搞 RSS，可以直接**删掉** README 里的 `~/latest-from-wiki` 整个区块，其他部分完全不受影响。

---

## 🔍 每个动态组件说明

| 组件 | 数据源 | 维护成本 |
|------|--------|---------|
| Visitor Counter (komarev.com) | 访问量实时计数 | 零 |
| Featured Projects Pin Cards | GitHub API 实时抓 star/fork/描述 | 零，新项目时改一次 |
| Stats Card | GitHub 账号统计 | 零 |
| Streak | 连续 commit 天数 | 零 |
| Top Languages | 所有仓库的语言占比 | 零 |
| Profile Summary | 综合数据卡 | 零 |
| Contribution Snake | 贡献热图做成贪吃蛇 | 零（Actions 每天跑） |
| Trophy | 成就徽章 | 零 |
| Blog Posts | chuiyu.wiki RSS | 零（Actions 每天抓） |

---

## ⚠️ 几个可能踩的坑

**1. Streak 图标不显示或报错**
`github-readme-streak-stats.herokuapp.com` 偶尔不稳定，可以换成备用域名：
```
https://streak-stats.demolab.com?user=ana52070...
```

**2. Pin Card 显示 "Error"**
说明仓库名拼错或仓库是 Private。确认仓库名大小写完全一致。

**3. 字体里的空格显示成加号**
`capsule-render` 的 text 参数需要 URL 编码，中文用 `%E4%BD%A0` 这种形式。我已经把你的名字编码好了，不用动。

**4. 想改配色**
所有卡片的 `theme=tokyonight` 都可以替换。可选：`dark`、`radical`、`merko`、`gruvbox`、`dracula`、`onedark`、`nord`、`solarized-dark`。

---

## 🎨 后续想改的话

- 换项目卡片：改 `~/featured-projects` 里的 `repo=xxx` 参数
- 换文案：`~/about` 和 `~/now-building` 是纯文本
- 删除某区块：整段删掉即可，各组件独立

---

## 📝 关于"不虚构"的设计取舍

README 里 `featured-projects` 只放了你**实际公开**的 4 个仓库（MCP_Control_STM32、jy901_for_ros2、web_onvif_modbus、STM32F103C8T6_FreeRTOS）。

你正在做的校园导航机器人、YOLO 云台、OpenClaw 这些放在了 `now-building` 里作为"进行中"的项目提及，没有伪装成已完成开源的工作——这样更真实，也避免了像 hazy1k 那种 "技能列表看起来很华丽但仓库里没有对应项目" 的落差感。

等这些项目开源了，替换上去就行。
