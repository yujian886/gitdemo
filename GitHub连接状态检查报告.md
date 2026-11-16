# GitHub 连接状态检查报告

**检查时间**：2026-11-12 16:05（GMT+8）
**更新时间**：2026-11-12 16:25（GMT+8），补充写权限实测与环境障碍记录
**检查对象**：WorkBuddy「GitHub」连接器 × 个人账号 `yujian886`
**检查方式**：真实调用 GitHub 接口（身份 / 仓库检索 / 文件读取 / PR 检索 / 写入尝试），非配置项推断

---

## 一、总体结论

> **读通道完全正常，写通道被连接器锁死为只读。** 可以立即用于读代码、查仓库、看 PR；但提交、推送、建 PR 均不可用，实测返回 `403 Resource not accessible by integration`。这是连接器的 GitHub App 权限声明所限，不是账号授权做错。
>
> 补建写通道的两条路当前都受阻：`gh` CLI 未安装且装不上（Homebrew 版本不兼容 + 下载通道被挡），本机也没有任何 GitHub 凭据。详见第八节。

状态：✅ 读取可用 / ❌ 写入不可用（连接器层面限制，非用户配置问题）

---

## 二、检查项明细

| # | 检查项 | 方法 | 结果 | 判定 |
|---|---|---|---|---|
| 1 | 连接器绑定 | 读取本地连接器状态 | GitHub 已绑定，凭证以 AES-256-GCM 加密落盘（最后一次更新 15:49） | ✅ 通过 |
| 2 | 身份认证（OAuth） | 调用 `get_me` | 返回 `yujian886`，id 64060894 | ✅ 通过 |
| 3 | 仓库读取权限 | 检索 `user:yujian886` | 自有仓库 11 个 + Fork 3 个 = 14 个，与账号 `public_repos: 14` 完全吻合 | ✅ 通过 |
| 4 | 代码内容读取 | 列出 `github-slideshow` 根目录 | 返回 15 个条目（Gemfile、_posts、index.html 等），结构完整 | ✅ 通过 |
| 5 | PR 读取 | 检索 `is:pr is:open author:yujian886` | 2 条未关闭 PR，含跨仓库贡献记录 | ✅ 通过 |
| 6 | 私有仓库访问 | 检索 `user:yujian886 is:private` | 0 条（该账号当前无私有仓库，非权限错误） | ➖ 不适用 |
| 7 | 写操作（commit / push / PR） | 尝试向 `gitdemo` 提交 `README.md` | `403 Resource not accessible by integration`，写入前即被拒绝，仓库未产生任何改动 | ❌ 不可用 |

---

## 三、账号信息

| 字段 | 值 |
|---|---|
| 用户名 | `yujian886` |
| 显示名 | yujian |
| 简介 | 业余rails工程师、科技爱好者 |
| 主页 | https://github.com/yujian886 |
| 注册时间 | 2020-04-21（约 6 年 7 个月） |
| 公开仓库 | 14 个（自有 11 + Fork 3） |
| Gist | 0 |
| 关注 / 粉丝 | 3 / 3 |
| 最后自有仓库推送 | 2023-04-12（`github-slideshow`） |

---

## 四、仓库清单（14 个）

### 自有仓库（11）

| 仓库 | 语言 | 默认分支 | 最后推送 | 开放 Issue | 备注 |
|---|---|---|---|---|---|
| github-slideshow | Ruby | master | 2023-04-12 | 3 | 唯一有近年活动的仓库；MIT 协议；已开 Pages |
| Computational-Thinking | — | master | 2021-05-01 | 0 | GitHub Learning Lab 练习 |
| rails104 | Ruby | master | 2021-04-10 | 0 | Rails 练习 |
| rails103 | Ruby | main | 2021-03-07 | 0 | Rails 练习 |
| yujian.github.io | — | gh-pages | 2021-02-17 | 1 | 个人主页，已开 Pages |
| Rails101 | Ruby | master | 2021-02-01 | 0 | Rails 练习 |
| second_app | Ruby | main | 2020-12-21 | 0 | Rails 练习 |
| github-pages-with-jekyll | — | main | 2020-12-14 | 0 | Jekyll 模板；MIT |
| first_app | Ruby | main | 2020-12-13 | 0 | 第一个 Rails 应用 |
| gitdemo | — | master | 2020-08-16 | 0 | 空仓库（size 0） |
| markdown-portfolio | — | master | 2020-04-23 | 1 | Markdown 练习；MIT |

### Fork 仓库（3）

| 仓库 | 上游说明 | 最后更新 |
|---|---|---|
| the-self-cultivation-of-leeks | 「愿你有平安的一生」 | 2020-04-23 |
| the-craft-of-selfteaching | 《自学是门手艺》 | 2026-09-21（上游同步） |
| job-listing | 招聘信息仓库 | 2021-04-17 |

### 画像小结

- 语言分布：Ruby 6 个、无主导语言 5 个、Fork 3 个 —— 典型的 **Rails 学习期（2020-12 ~ 2021-04）** 仓库群。
- 全部仓库 0 star、0 fork，无协作型项目，属于纯个人学习/练习空间。
- 唯一长期在用的是 `github-slideshow`（Pages 站点）和 `yujian.github.io`。
- 近 2 年半没有自有仓库的代码提交，`the-craft-of-selfteaching` 的 2026-09 更新来自上游同步。

---

## 五、当前可用的操作

已验证可用：

- 「看一下我 `rails104` 这个项目是干什么的，帮我写份 README」
- 「列出我所有仓库里还有未关闭 Issue 的，整理成表格」
- 「把我这 11 个仓库按语言和时间做个分析」
- 「读一下 `github-slideshow/_posts` 里最新的一篇文章」

当前**不可用**的操作（写类）：

- 「把这个项目 push 到 xxx 仓库」
- 「给 xxx 仓库提个 PR」
- 「帮我在 xxx 仓库改一下 README」

原因见第六、八节。

---

## 六、发现的问题与建议

### 1. 写权限被连接器锁死为只读（核心问题）

向 `gitdemo` 提交首个 `README.md` 的实测结果：

```
PUT /repos/yujian886/gitdemo/contents/README.md
403 Resource not accessible by integration
```

`integration` 指 GitHub App 的安装令牌。关键佐证：同一时刻对同一仓库的**读**操作完全正常（列分支返回空数组）。说明 App 已正确安装到 `gitdemo`，但其权限声明只有 contents 读，没有写。

这不是授权时点错了，而是 WorkBuddy 这个 GitHub 连接器的 App 本身只申请了读权限——设计上写操作要靠本地 `gh` CLI 兜底。而 `gh` 在本机不存在（见第八节，且装不上）。结论：**当前这台机器上不存在任何可写入 GitHub 的通道。**

### 2. 补写通道的可行方案（三选一，均需用户动手）

| 方案 | 做法 | 代价 |
|---|---|---|
| PAT（最快） | 建 Fine-grained token，限 `gitdemo` 仓库 + Contents/Pull requests 读写 + 7 天有效期；push 用 `git push https://<token>@...`，PR 用 REST API `POST /repos/{owner}/{repo}/pulls` | token 会经过对话，用完须立即删除 |
| 自行执行 | 由 WorkBuddy 输出完整 git 命令与 PR 的 curl 命令，用户在本地终端执行 | 不涉及凭据传递，但需手工粘贴 |
| 修复环境后装 gh | 先修 Homebrew（需升级到支持 macOS 27 的版本），再 `brew install gh && gh auth login` | 成本最高，但一劳永逸 |

### 3. 授权范围可能过宽（提示，非问题）

GitHub App 的 Repository access 如果选的是「All repositories」，那么你**将来**新建的仓库也会自动对 WorkBuddy 可见。当前你只有公开仓库、无私有仓库，风险很低；但以后如果有私有仓库，建议改成「Only select repositories」按需勾选。

### 4. 一点观察

你的 GitHub 几乎停在 2021 年 4 月。如果接上连接器是想重新开始写点东西，`github-slideshow` 和 `yujian.github.io` 这两个已经有 Pages 配置的仓库是成本最低的起点；`gitdemo` 是空仓库，适合当试验田。

---

## 七、排查路径（备用）

如果后续出现连接异常，按顺序看：

| 位置 | 用途 |
|---|---|
| `~/.workbuddy/connectors/cb4912a5-.../connector-states.v3.json` | 连接器绑定状态与加密凭证 |
| `~/.workbuddy/logs/daemon.log` | 搜 `server-side-token-check`，看 token 校验结果 |
| `~/.workbuddy/logs/network-failover.jsonl` | 网络与故障转移，排除代理/网络问题 |
| GitHub → Settings → Applications → Installed GitHub Apps | 看 CodeBuddy-Connector 是否还在、仓库范围是否正确 |

---

## 八、环境障碍记录（2026-11-12 实测）

为补建写通道，尝试安装 GitHub CLI，两条路依次失败。

### 障碍 A：Homebrew 版本不兼容本机 macOS

`brew --version` 本身能执行（Homebrew 4.4.5，`/opt/homebrew`），但任何实际操作都崩溃：

```
/opt/homebrew/Library/Homebrew/macos_version.rb:53:in `initialize':
  unknown or unsupported macOS version: "27.0" (MacOSVersion::Error)
```

系统报告的 macOS 版本为 **27.0**，Homebrew 4.4.5 不认识这个版本号。该错误在 `brew.rb` 加载阶段抛出，因此 `brew install`、`brew update` 等全部命令均不可用，**无法用 brew 自我修复**。

首次尝试还出现过 `Exit 137 / SIGTERM`（沙箱内执行的进程被终止），确认属环境限制而非命令写错。

### 障碍 B：GitHub 文件下载通道被阻断

绕过 Homebrew、直接从 GitHub Releases 取二进制包，连通性测试结果：

| 地址 | HTTP 状态 | 判定 |
|---|---|---|
| `github.com/cli/cli/releases/download/v2.103.3/gh_..._arm64.zip` | 502 / 000（连接重置） | ❌ 不可下载 |
| `raw.githubusercontent.com` | 000 | ❌ 不可达 |
| `api.github.com`（REST API） | 200 | ✅ 正常 |
| `objects.githubusercontent.com` | 404（根路径正常响应） | ✅ 可达 |

特征很明确：**API 通、文件下载被挡**。`curl` 下载在 180s 超时后终止（exit 28），目录内无残留文件。

未采用第三方镜像下载可执行文件——来源无法验证的二进制存在供应链风险，不值得为一个可替代的工具承担。

### 当前环境结论

| 项 | 状态 |
|---|---|
| 可执行文件 `gh` | 未安装 |
| Homebrew 可用性 | ❌ 完全不可用（版本不兼容 macOS 27.0） |
| 本机 GitHub 凭据 | ❌ 无（无 credential helper，keychain 无 github.com 记录） |
| git 本体 | ✅ 可用（git 2.50.1，Apple Git-155） |
| git 仓库 | 当前工作目录非 git 仓库（`fatal: not a git repository`） |

**净结果：没有安装任何软件，没有修改任何系统配置，`gitdemo` 仍未产生任何提交或分支。**

---

## 九、后续复测记录（2026-11-16）

环境已发生变化，第八节的障碍在本日全部解除，但**连接器的只读限制未变**。

### 环境变化

| 项 | 11-12 | 11-16 |
|---|---|---|
| Homebrew | ❌ 崩于 macOS 27.0（4.4.5） | ✅ 7.0.13 正常 |
| `gh` CLI | ❌ 未安装 | ✅ v2.104.0（`/opt/homebrew/bin/gh`） |
| gh 登录 | — | ✅ 设备码登录，身份 `yujian886` |
| 本机 GitHub 凭据 | ❌ 无 | ✅ 由 gh 凭据助手托管 |

### 复测项一：连接器写入（无变化）

```
PUT /repos/yujian886/gitdemo/contents/README.md
403 Resource not accessible by integration
```

结论不变：**MCP 连接器通道仍为只读**，这是 GitHub App 的权限声明所致，与本机环境无关。

### 复测项二：gh CLI 本地通道（全部通过）

| 动作 | 结果 |
|---|---|
| 初始化仓库并推送首个提交到 `master` | ✅ 提交 `23f504b`，`[new branch] master -> master` |
| 创建分支 `add-github-status-report` 并推送报告 | ✅ 提交 `5bf6886` |
| 创建 Pull Request | ✅ PR #1：https://github.com/yujian886/gitdemo/pull/1 |

### 结论修正

读写能力应分开表述：

- **读**：MCP 连接器通道，✅ 可用，无需额外配置
- **写**：`gh` CLI 本地通道，✅ 可用（需 gh 已安装并登录）；MCP 连接器通道 ❌ 不可用

也就是说，WorkBuddy 里的写操作能否成功，取决于本机有没有 `gh`，而不是取决于连接器授权。

---

*本报告由 WorkBuddy 自动生成，所有数据均来自 GitHub 实时接口调用与本地命令实测，未做人工补全。*
