# RaysBlog Updater

---start---
## 目录(2026年05月15日更新)
[Guerilla Open Access Manifesto](https://rayyu.me/p/2026-01-04-guerilla-open-access-manifesto/)

[DIY全屋智能：我的米家智能家居方案设计与踩坑全记录](https://rayyu.me/p/2025-11-03-diy-smart-home/)

---end---

## Intro
This repository offers a convenient way to manage your WordPress in the Hexo way, which supports using Github Actions to automatically upload and update articles to WordPress. 

Compared with orignal repository, following features added:

- Added support for [CloudFlare-ImgBed](https://github.com/MarSeventh/CloudFlare-ImgBed), a self-hosting image hosting service.
- Resolved the 403 error when updating posts, which caused by Cloudflare WAF and anti-bot service.

## Deployment
1. fork this project;
2. Set up environment variables in forked project's page (settings-secrets and variables);
3. Input listed environment variables. Explanation of environment variables lists blow:

| Env Variables  | Explanation | Where can find |
| ------------- | ------------- | ------------- |
| CF_ACCOUNT_ID  | Your Cloudflare Account ID | Cloudflare dashboard → Account home → Three dot behind the title → Copy account id |
| CF_ZONE_ID  | Your Domain Managed by Cloudflare  | Your WordPress Host Domain|
| CF_TOKEN_GH_ACTIONS  | Your Cloudflare Account API Token  | Your Cloudflare API Token, Which has listed permissions:Account.Account Filter Lists, Zone.Bot Management,  Zone.Zone WAF, Zone.Zone Settings,Zone.Zone Settings.|
| USERNAME  | Your WordPress Account Username  | As it is|
| PASSWORD | Your WordPress Account Password  | As it is|
| XMLRPC_PHP | Your WordPress XMLRPC Address  | e.g. https://YourWordPressDomain.com/xmlrpc.php|
| IMAGE_HOSTING_URL | Image Hosting Service Url   | Depends on the Img Hosting Service you use. If you use [CloudFlare-ImgBed](https://github.com/MarSeventh/CloudFlare-ImgBed), the link would be https://YourImgHostDomain.com/upload. |
| IMAGE_HOSTING_SECRET_TOKEN | Image Hosting Service Upload API Token | Depends on the Img Hosting Service you Use. If you use [CloudFlare-ImgBed](https://github.com/MarSeventh/CloudFlare-ImgBed), You can set up the token in System Settings → Security Settings. |

4. Clone your forked project locally, and delete my articles in your project.
5. Write articles in `_posts/`, articles' .md file should be named as YourArticleName.md, images in articles should be placed in related path as YourArticleName.assets.
6. Commit and push — the GitHub Action will sync them to WordPress automatically, then enjoy.

## Note
1. If there is error during login process in Action, try to disable Wordfence's limitation of XMLRPC (Wordfence-all settings-update login security options-uncheck 'Disable XML-RPC authentication' and select SKIPPED in 'Require 2FA for XML-RPC call authentication').
2. Try to use application password of WordPress, which can be enabled in profile page.
3. If Wordfence blocks Cloudflare's ips, add listed code in wp-config.pho before '/* That's all, stop editing! */'.
```
if (isset($_SERVER['HTTP_CF_CONNECTING_IP'])) {
    $_SERVER['REMOTE_ADDR'] = $_SERVER['HTTP_CF_CONNECTING_IP'];
}
```
---
## 简介

本项目提供了一种用 Hexo 的方式管理 WordPress 的便捷方案，支持通过 GitHub Actions 自动将文章上传和更新到 WordPress。

相比原项目，新增了以下功能：

- 支持 [CloudFlare-ImgBed](https://github.com/MarSeventh/CloudFlare-ImgBed) 自部署图床服务。
- 解决了 Cloudflare WAF 和反爬机制导致的 403 错误。

## 部署

1. Fork 本项目；
2. 在 Fork 后的项目页面设置环境变量（Settings → Secrets and variables）；
3. 填入以下环境变量，说明如下：

| 环境变量  | 说明 | 获取方式 |
| ------------- | ------------- | ------------- |
| CF_ACCOUNT_ID  | Cloudflare 账户 ID  | Cloudflare 首页 → 账户主页 → 标题旁三个点 → 复制账户 ID |
| CF_ZONE_ID  | WordPress 域名在 Cloudflare 的 Zone ID  | Cloudflare 首页 → 选择你的域名 → 概述页面右侧栏 Zone ID |
| CF_TOKEN_GH_ACTIONS  | Cloudflare API Token  | Cloudflare → My Profile → API Tokens → 创建 Token，需包含以下权限：Account.Account Filter Lists、Zone.Bot Management、Zone.Zone WAF、Zone.Zone Settings |
| USERNAME  | WordPress 后台登录用户名  | 即你的 WordPress 用户名 |
| PASSWORD  | WordPress 后台登录密码  | 即你的 WordPress 密码 |
| XMLRPC_PHP  | WordPress XML-RPC 接口地址  | 格式：`https://你的WordPress域名/xmlrpc.php` |
| IMAGE_HOSTING_URL  | 图床上传接口地址  | 取决于你使用的图床服务。若使用 [CloudFlare-ImgBed](https://github.com/MarSeventh/CloudFlare-ImgBed)，地址为 `https://你的图床域名/upload` |
| IMAGE_HOSTING_SECRET_TOKEN  | 图床上传密钥  | 取决于你使用的图床服务。若使用 [CloudFlare-ImgBed](https://github.com/MarSeventh/CloudFlare-ImgBed)，在系统设置 → 安全设置中配置 |

4. 克隆 Fork 后的项目到本地，删除 `_posts/` 目录下的示例文章；
5. 在 `_posts/` 目录下编写文章，Markdown 文件命名为 `文章名.md`，文章中的图片放在对应的 `文章名.assets` 目录下；
6. 提交并推送 — GitHub Actions 会自动同步到 WordPress，enjoy。
## 提示
1. 如果 GitHub Actions 运行过程中出现登录错误，请检查 Wordfence 的 XML-RPC 限制设置（Wordfence → All Options → Login Security → 取消勾选「Disable XML-RPC authentication」，并将「Require 2FA for XML-RPC call authentication」设为 SKIPPED）；
2. 建议使用 WordPress 应用密码代替登录密码，可在用户个人资料页面生成；
3. 如果 Wordfence 拦截了 Cloudflare 的 IP，请在 `wp-config.php` 的 `/* That's all, stop editing! */` 注释之前添加以下代码，使 WordPress 能正确识别真实客户端 IP：
```// 在 wp-config.php 的 "That's all" 注释之前加
if (isset($_SERVER['HTTP_CF_CONNECTING_IP'])) {
    $_SERVER['REMOTE_ADDR'] = $_SERVER['HTTP_CF_CONNECTING_IP'];
}
```