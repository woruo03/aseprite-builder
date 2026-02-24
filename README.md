# Aseprite Builder for Debian 13

[![Build Aseprite for Debian 13](https://github.com/woruo03/aseprite-builder/actions/workflows/aseprite-build-deploy.yml/badge.svg)](https://github.com/woruo03/aseprite-builder/actions/workflows/aseprite-build-deploy.yml)

## ⚠️ 重要法律声明

**本项目仅用于个人学习和研究目的**。为了尊重Aseprite的版权和许可条款，本项目采取以下措施：

### 关键限制
1. **禁止公开发布**：构建的Aseprite二进制文件**绝对不能公开发布**，只能保持在GitHub Releases的草稿状态
2. **仅供个人使用**：构建产物仅限构建者个人使用，不得分发或共享
3. **必须fork使用**：用户必须fork本仓库到自己的GitHub账户，然后使用工作流构建供个人使用
4. **遵守EULA**：使用前必须阅读并遵守[Aseprite最终用户许可协议(EULA)](https://github.com/aseprite/aseprite/blob/main/EULA.txt)

### 为什么需要这些限制？
- Aseprite是商业软件，拥有严格的许可条款
- 公开发布构建的二进制文件可能侵犯Aseprite的版权
- 这些限制确保项目在法律允许的范围内运作

## 许可证

本项目代码基于MIT许可证分发。

## 使用方式（正确流程）

### 步骤1：Fork仓库
1. 点击GitHub页面右上角的"Fork"按钮
2. 将本仓库fork到你自己的GitHub账户

### 步骤2：启用Actions
1. 在你的fork仓库中，进入"Actions"标签页
2. 启用GitHub Actions工作流

### 步骤3：触发构建
1. 进入`.github/workflows/aseprite-build-deploy.yml`文件
2. 点击"Run workflow"按钮
3. 可以选择配置参数或使用默认值

### 步骤4：获取构建产物
1. 构建完成后，进入"Releases"页面
2. **重要**：构建产物将以**草稿**形式存在
3. **不要点击"Publish release"** - 保持草稿状态
4. 从草稿中下载构建产物供个人使用

### 步骤5：安装与使用

#### 5.1 下载构建产物
1. 构建完成后，进入GitHub Releases页面
2. 找到草稿（Draft）版本的发布
3. 下载以下文件之一：
   - `aseprite_*.deb` - Debian软件包（仅amd64架构）
   - `aseprite-*-debian13-x64.tar.xz` - 通用Linux归档（仅amd64架构）

   **重要提示**：所有构建产物仅支持 **amd64 (x86_64)** 架构，不适用于ARM架构设备（如树莓派）。

#### 5.2 文件格式说明

**`.deb`包**：
- **适用系统**：Debian 13、Ubuntu 22.04+ 及其衍生发行版
- **架构限制**：仅支持 **amd64 (x86_64)** 架构，不包含ARM架构（如树莓派）
- **优点**：
  - 自动安装到系统目录（`/usr/bin/aseprite`）
  - 创建桌面快捷方式和菜单项
  - 自动解决依赖关系
  - 可通过`apt`管理

**`.tar.xz`归档**：
- **适用系统**：任何Linux发行版（包括Arch、Fedora、openSUSE等）
- **架构限制**：仅支持 **amd64 (x86_64)** 架构，不包含ARM架构
- **优点**：
  - 无需root权限即可运行
  - 便携式，可放在任意目录
  - 包含所有运行时数据文件
  - 不修改系统文件

## 项目概述

这是一个自动化构建工具，用于在Debian 13系统上构建[Aseprite](https://www.aseprite.org/)像素艺术编辑器。项目通过GitHub Actions工作流自动从上游Aseprite仓库获取最新版本源代码，编译并打包为`.tar.xz`归档文件和`.deb`软件包。

## 功能特性

- **安全构建**：构建产物自动保持草稿状态，避免意外公开发布
- **自动构建**：自动检测Aseprite最新版本并下载源代码
- **依赖管理**：自动下载并配置Skia图形库（`aseprite-m124`分支）
- **多格式打包**：生成适用于Debian 13的`.tar.xz`和`.deb`包
- **桌面集成**：自动创建桌面快捷方式、图标和应用程序菜单项
- **GitHub Actions集成**：支持标签触发和手动触发构建

## GitHub Actions工作流

### 触发方式

1. **手动触发**：通过GitHub Actions界面手动触发工作流（主要使用方式）
2. **标签触发**：当仓库中有新标签推送时自动运行

### 工作流输入参数

手动触发时，可以配置以下参数：

| 参数 | 描述 | 默认值 |
|------|------|--------|
| `aseprite_tag` | 上游Aseprite标签（空值表示最新版本） | "" |
| `skia_release_tag` | 上游aseprite/skia发布标签或主要前缀（如m124） | "m124" |
| `deb_revision` | Debian修订后缀 | "1" |
| `release_tag` | 本仓库中的发布标签（空值表示自动生成） | "" |
| `release_name` | 本仓库中的发布名称（空值表示自动生成） | "" |

### 构建过程

工作流执行以下步骤：

1. **解析上游元数据**：获取Aseprite和Skia的最新版本信息
2. **下载源代码**：下载Aseprite源代码和预编译的Skia库
3. **构建Aseprite**：使用CMake和Ninja编译Aseprite
4. **打包**：创建`.tar.xz`归档文件和`.deb`软件包
5. **草稿发布**：将构建产物以草稿形式上传到GitHub Releases

## 法律合规性检查

### 允许的行为
- ✅ Fork仓库到个人账户
- ✅ 使用GitHub Actions构建供个人使用
- ✅ 从草稿中下载构建产物
- ✅ 在个人设备上安装和使用

### 禁止的行为
- ❌ 公开发布构建产物
- ❌ 分发或共享构建的二进制文件
- ❌ 用于商业目的
- ❌ 修改Aseprite源代码后重新分发


### 常见问题

1. **构建失败**：检查GitHub Actions日志中的错误信息
2. **依赖问题**：确保所有构建依赖已正确安装
3. **Skia版本不匹配**：确保使用正确的Skia分支（默认为`m124`）
4. **无法找到构建产物**：检查Releases页面的草稿部分

### 获取帮助

如果在使用过程中遇到问题，请：
1. 查看GitHub Actions构建日志
2. 在GitHub Issues中提交问题

## 相关链接

- [Aseprite官方网站](https://www.aseprite.org/)
- [Aseprite GitHub仓库](https://github.com/aseprite/aseprite)
- [Skia图形库](https://github.com/aseprite/skia)
- [GitHub Actions文档](https://docs.github.com/en/actions)
- [Aseprite EULA](https://github.com/aseprite/aseprite/blob/main/EULA.txt)

---

*最后更新: 2026-02-24*
*重要提示：请严格遵守上述使用限制，尊重软件版权和许可条款*
