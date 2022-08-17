---
title: 如何将自己的依赖包发布到 Composer Packagist
date: 2021-08-10 17:09:51.828
updated: 2021-08-10 20:20:27.681
url: publish-composer-package-to-packagist
categories: 
- PHP
tags: 
- PHP
- Composer
---

# 准备工作
- Github 账号
- Packagist 账号

# 创建 git 仓库
GitHub 创建仓库并克隆到本地。
```
git clone git@github.com:Great233/url.git
```
# composer 初始化
```
composer init
```
输入包名称、描述、作者、协议、依赖等信息，回车结束。最终会生成`composer.json`配置文件，文件内容如下：
```json
{
    "name": "abtswiath/url",
    "description": "A library to parse URL, support parameters with the same name",
    "type": "library",
    "require": {
        "php": ">=7.2"
    },
    "version": "1.0.0",
    "license": "MIT",
    "autoload": {
        "psr-4": {
            "Url\\": "src/Url/"
        }
    },
    "authors": [
        {
            "name": "Great"
        }
    ],
    "require-dev": {
        "phpunit/phpunit": "^9.5"
    },
    "support": {
        "issues": "https://github.com/Great233/url/issues",
        "source": "https://github.com/Great233/url"
    }
}
```
# 提交代码至 Github
代码编写完成并且测试无误之后提交到 Github。

#### 以下步骤可选：
创建标签以标记版本，如：
```
git tag -a 1.0.0 -m 'Release 1.0.0'
```
推送标签至 Github：
```
git push origin 1.0.0
```
Github 创建新的 Release 版本，版本号参考[语义版本控制](https://semver.org/)
# 发布到Packagist
登录 Packagist，打开[提交页面](https://packagist.org/packages/submit)，输入 Github 存储库 URL，点击 Check 按钮检出仓库，系统会根据`composer.json`文件内容自动设置包的相关信息。
依赖包发布之后可设置自动更新，Packagist `Settings` 中按照说明操作即可。
# 使用依赖包
发布到 Packagist 之后就可以在自己项目中安装该依赖包：
```
composer require abtswiath/url
```
**注：**
如果没有创建标签发布版本，需要指定 dev：
```
composer require 'abtswiath/url @dev'
```
