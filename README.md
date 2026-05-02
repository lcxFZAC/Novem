# 玖镇秘典 Wiki 

基于 GitHub Pages 托管的世界知识库，支持 GitHub 账号登录与权限管理。

## 目录结构

```
jiuzhen-wiki/
├── index.html                    # 主页面（含 __USERS_INJECT__ 占位符）
├── config.json                   # 站点公开配置（不含用户列表）
├── data/
│   └── pages.json                # Wiki 页面数据
└── .github/
    └── workflows/
        └── deploy.yml            # 构建时注入用户列表 → 部署
```

## 安全设计

用户权限表**不存在于仓库任何文件中**，存放在 GitHub 仓库的 **Actions Secret** 里。  
构建时 GitHub Actions 将 Secret 注入编译产物，源码公开也不会暴露谁有权限。

## 权限角色

| 角色 | 说明 | 编辑 | 删除 | 新建 |
|------|------|------|------|------|
| admin（典守） | 完全权限 | ✅ | ✅ | ✅ |
| editor（笔吏） | 可读写不可删 | ✅ | ❌ | ✅ |
| viewer（访客） | 只读（默认） | ❌ | ❌ | ❌ |

## 部署步骤

### 1. 创建 GitHub 仓库

New repository → 名称 `jiuzhen-wiki` → **Public** → Create

### 2. 修改 config.json

把 `YOUR_GITHUB_USERNAME` 替换成你的 GitHub 用户名：

```json
{
  "site": {
    "repo": "你的用户名/jiuzhen-wiki"
  }
}
```

### 3. 添加 WIKI_USERS Secret（核心步骤）

进入仓库 → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

- Name：`WIKI_USERS`  
- Secret：填入 JSON 格式的用户列表，例如：

```json
[
  {"github_username": "你的用户名", "role": "admin"},
  {"github_username": "朋友的用户名", "role": "editor"}
]
```

注意：必须是**单行 JSON**（或多行，GitHub 会正确处理）。

### 4. 推送代码

```bash
cd jiuzhen-wiki
git init
git add .
git commit -m "🏮 初始化玖镇秘典"
git remote add origin https://github.com/你的用户名/jiuzhen-wiki.git
git branch -M main
git push -u origin main
```

### 5. 开启 GitHub Pages

仓库 **Settings** → **Pages** → Source 选 `GitHub Actions` → 保存  
等待约 1 分钟，访问 `https://你的用户名.github.io/jiuzhen-wiki/`

### 6. 用户如何登录

1. 访问网站，点右上角 **GitHub 登录**
2. 前往 [github.com/settings/tokens/new](https://github.com/settings/tokens/new)，勾选 **repo** 权限，生成 Token
3. 粘贴到登录框，系统自动识别角色

## 修改用户权限

在 GitHub 仓库 **Settings → Secrets → WIKI_USERS** 里直接编辑 JSON，  
然后触发一次部署（push 任意文件，或手动 Actions → Run workflow）即可生效。

> Token 仅存在用户自己浏览器的 localStorage，不经过任何服务器。
