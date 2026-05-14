# 玖镇秘典 Wiki 

基于 GitHub Pages 托管的世界知识库，支持 GitHub 账号登录与权限管理。

## 仓库命名含义
拉丁语“九”（novem），可独立作为镇名，简洁对应“玖镇”

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

## 权限角色

| 角色 | 说明 | 编辑 | 删除 | 新建 |
|------|------|------|------|------|
| admin（典守） | 完全权限 | ✅ | ✅ | ✅ |
| editor（笔吏） | 可读写不可删 | ✅ | ❌ | ✅ |
| viewer（访客） | 只读（默认） | ❌ | ❌ | ❌ |


##用户如何登录

1. 访问网站，点右上角 **GitHub 登录**
2. 前往 [github.com/settings/tokens/new](https://github.com/settings/tokens/new)，勾选 **repo** 权限，生成 Token
3. 粘贴到登录框，系统自动识别角色

## 修改用户权限

在 GitHub 仓库 **Settings → Secrets → WIKI_USERS** 里直接编辑 JSON，  
然后触发一次部署（push 任意文件，或手动 Actions → Run workflow）即可生效。
> Token 仅存在用户自己浏览器的 localStorage，不经过任何服务器。
