# 玖镇秘典 Wiki

基于 GitHub Pages 托管的世界知识库，支持 GitHub 账号登录与权限管理。

## 目录结构

```
jiuzhen-wiki/
├── index.html          # 主页面（前端全部逻辑）
├── config.json         # 站点配置与用户权限
├── data/
│   └── pages.json      # Wiki 页面数据（通过 GitHub API 读写）
└── .github/
    └── workflows/
        └── deploy.yml  # 自动部署到 GitHub Pages
```

## 权限角色

| 角色 | 说明 | 编辑 | 删除 | 新建 |
|------|------|------|------|------|
| admin（典守） | 管理员，完全权限 | ✅ | ✅ | ✅ |
| editor（笔吏） | 编辑者，可读写但不可删除 | ✅ | ❌ | ✅ |
| viewer（访客） | 只读，未在列表中的用户默认此角色 | ❌ | ❌ | ❌ |

## 添加/修改用户权限

编辑 `config.json` 中的 `users` 数组：

```json
{
  "users": [
    {
      "github_username": "用户名",
      "role": "admin",
      "display_name": "显示名称"
    },
    {
      "github_username": "另一个用户名",
      "role": "editor",
      "display_name": "编辑者"
    }
  ]
}
```

## 用户如何登录

1. 访问网站后点击右上角 **GitHub 登录**
2. 前往 [GitHub Token 页面](https://github.com/settings/tokens/new) 创建 Token
3. 勾选 **repo** 权限后生成
4. 粘贴 Token 到登录框，系统自动识别权限

> Token 仅存储在用户自己浏览器的 localStorage 中，不会上传到任何服务器。

## 部署步骤

详见下方的部署文档。
