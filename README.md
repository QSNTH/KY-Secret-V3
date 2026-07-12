# KY Secret V3 · WebEnd

KY Secret V3 前端页面，校园匿名投稿墙的用户端与管理端界面。

纯静态 HTML/CSS/JS，无框架依赖，通过 `config.json` 动态连接后端 API。

## 目录结构

```
WebEnd/
├── index.html              # 用户投稿页面
├── result.html             # 投稿状态查询页面
├── config.json             # API 端点配置
├── admin/                  # 管理端页面
│   ├── index.html          # 管理员登录
│   ├── dashborad.html      # 管理面板（统计、硬件、日志、截图）
│   ├── review.html         # 审核工作台（投稿审核、批量操作）
│   └── review_view.html    # 审核界面预览（静态演示）
├── status/                 # 公共服务状态页面
│   ├── index.html          # 服务状态 + 投稿统计
│   └── history.html        # 历史事件记录
└── pdf/                    # PDF 文件目录
```

## 页面说明

### `index.html` — 匿名投稿
- 标题（必填，≤80字符）、内容（≤2000字符）、标签（≤20字符）
- 支持上传图片
- 输入过滤：中文、英文、数字、常用标点、Emoji
- 提交后跳转至 `result.html?uuid=...`

### `result.html` — 状态查询
- 通过 UUID 查询投稿审核状态和发布结果
- 展示生成的文字转图片、用户上传图片
- 支持复制链接分享

### `admin/index.html` — 管理员登录
- Bearer Token 认证流程
- 登录成功跳转至 `dashborad.html`

### `admin/dashborad.html` — 管理面板
- 投稿统计概览
- 系统硬件监控（CPU、GPU、内存、磁盘）
- 服务日志查看
- 服务器截图

### `admin/review.html` — 审核工作台
- 左右分栏：投稿列表 + 详情面板
- 按状态筛选（待审核/已通过/已驳回/全部）
- 关键词搜索（300ms 防抖）
- 通过 / 驳回 / 推送 / 重推
- **多选功能**：Ctrl+点击 切换选取、Shift+点击 范围选取
- **批量操作**：选取后底部浮出「通过所选」「驳回所选」「取消选取」

### `status/index.html` — 公共服务状态
- 投稿总数、通过数、API 健康状态

### `status/history.html` — 历史事件
- 历史发布记录时间线

## 配置

`config.json` 定义后端 API 地址和各端点路径：

```json
{
  "backend": "http://127.0.0.1:8000",
  "api": "/api",
  "submit": "/submit",
  "lookup": "/lookup/{uuid}",
  ...
}
```

前端加载逻辑：
1. 先尝试从 GitHub Raw 获取 `config.json`（生产环境）
2. 失败则回退到本地 `config.json`（本地开发）
3. 都失败则使用硬编码的默认隧道地址

## 部署

静态文件可直接部署到任意 HTTP 服务器：

- **本地开发**：直接打开 HTML 文件或使用 Live Server
- **生产部署**：GitHub Pages / Cloudflare Pages / Nginx 等

需确保 `config.json` 中的 `backend` 指向正确的后端服务地址。
