# 极简静态博客 + Decap CMS

一个无后台的极简博客：内容存 GitHub，Cloudflare Pages 自动构建发布，`/admin` 用 Decap CMS 可视化编辑。

## 结构

```
content/posts/        # Markdown 文章
public/admin/         # Decap CMS
src/pages/            # 页面
```

## 本地开发

```sh
npm install
npm run dev
```

文章目录：`content/posts/*.md`

### 本地 CMS（可选）

Decap CMS 本地开发需要使用 `local_backend`，已准备配置文件：

1. 复制 `public/admin/config.local.yml` 覆盖 `public/admin/config.yml`
2. 运行本地代理：
   ```sh
   npx decap-server
   ```
3. 访问 `http://localhost:4321/admin`

生产环境请保留 `public/admin/config.yml`（不启用 `local_backend`）。

## 生产部署（Cloudflare Pages）

- 连接 GitHub 仓库
- 构建命令：`npm run build`
- 输出目录：`dist`

每次 CMS 保存会提交到 `main` 分支，Pages 自动构建发布。

## GitHub OAuth（Decap CMS）

1. 在 GitHub 创建 OAuth App
2. 回调地址：`https://<你的域名>/admin/`
3. 将 `client_id` 填入 `public/admin/config.yml` 的 `app_id`

## Cloudflare Access 保护 `/admin`

在 Zero Trust 创建 Self-hosted Application：

- Application domain: `https://<你的域名>`
- Path: `/admin*`
- 允许的身份：你的邮箱/团队

这样可以确保只有授权用户能进入 CMS。
