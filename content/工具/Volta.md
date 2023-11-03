---
title: Volta 前端工具链版本管理工具的使用
tags:
  - 前端
  - 开发工具
  - volta
---
Volta 是一款使用 `rust` 语言开发的前端工具链管理工具。以下是它的官网和源码仓库地址：

🏠 官网： [Volta](https://volta.sh/)

🔗 Github: [Volta](https://github.com/volta-cli/volta)

## ⬇️ 安装

Windows 安装建议开启开发者模式。

[Release](https://github.com/volta-cli/volta/releases) -> 选择  `volta-${version}-windows.zip`  下载，解压到安装目录（如：D:/volta），并且添加到 `PATH` 环境变量中。

新建环境变量 `VOLTA_HOME` ，用于存储 Volta 数据文件的文件夹（如：D:/volta-data），并在 `PATH` 环境变量中添加 `%VOLTA_HOME%\bin` .

## 🏗️ 配置

### nodejs 国内镜像

Volta 提供了 [hooks](https://docs.volta.sh/advanced/hooks/) 功能，通过配置 hooks 来切换镜像下载地址。

- [清华镜像 nodejs-release](https://mirrors.tuna.tsinghua.edu.cn/help/nodejs-release/)

 > 创建或编辑 `~/.volta/hooks.json` (Linux/MacOS)，或 `%LOCALAPPDATA%\Volta\hooks.json` (Windows)，

或者直接在 Volta 的安装目录下创建 `hooks.json` 文件，并保存如下内容（注意：windows的文件后缀是zip或者7z）：
```json
{
  "node": {
    "index": {
      "template": "https://mirrors.tuna.tsinghua.edu.cn/nodejs-release/index.json"
    },
    "distro": {
      "template": "https://mirrors.tuna.tsinghua.edu.cn/nodejs-release/v{{version}}/node-v{{version}}-{{os}}-{{arch}}.zip"
    }
  }
}
```

### [支持pnpm](https://docs.volta.sh/advanced/pnpm)

添加环境变量 ``VOLTA_FEATURE_PNPM``, 值为 `1` 

## 💻 命令

📃 [官方文档](https://docs.volta.sh/reference/)

介绍几个常用的命令：
- volta fetch 提前把工具链下载到本地缓存，供以后脱机使用。
- volta install 下载工具链，切换版本。
- volta pin 会将当前环境中的工具链版本写入到 `package.json` 文件中，就这一个功能。
- volta list 展示当前环境中工具链的版本。
- volta which 查询使用的工具链路径。
- volta setup 会先在当前用户的配置中查找Volta的环境变量，然后启动。可以使用 `volta setup --verbose` 查看当前的一些配置。
- volta run 指定工具链版本运行，而不是使用默认或者 pin 的版本。

### 查看当前工具链版本

```bash
volta list
```

### 下载或者切换版本

```bash
volta install node@lts
```
