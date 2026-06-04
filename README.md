# Aseprite Builder for Linux

[![Build Aseprite for Linux](https://github.com/woruo03/aseprite-builder/actions/workflows/aseprite-build-deploy.yml/badge.svg)](https://github.com/woruo03/aseprite-builder/actions/workflows/aseprite-build-deploy.yml)

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

## 汉化包获取

Aseprite 提供了官方认可的社区翻译，可以通过以下方式获取：

- **Weblate 翻译平台**：https://hosted.weblate.org/projects/aseprite/-/zh_Hans/

## 快速开始指南

### 最简单的使用方式（推荐）

```
1. Fork 本仓库到你的账户
2. 进入 Actions → Build Aseprite for Linux
3. 点击 Run workflow → Run workflow（使用默认配置）
4. 等待构建完成 → Releases 页面下载草稿中的产物
```

**就是这样！** 构建会自动使用 Aseprite 最新版本。

### 安装与使用
1. 构建完成后，进入GitHub Releases页面
2. 找到草稿（Draft）版本的发布
3. 下载以下文件之一：
   - `aseprite-*-linux-x64.tar.xz` - 通用Linux归档（仅amd64架构）
   - `aseprite_*.deb` - Debian软件包（仅amd64架构）
   - `aseprite-bin-*-x86_64.pkg.tar.zst` - Arch Linux软件包（仅x86_64架构）

   **重要提示**：所有构建产物仅支持 **amd64 (x86_64)** 架构，不适用于ARM架构设备（如树莓派）。

#### 文件下载
1. 构建完成后，进入GitHub Releases页面
2. 找到**草稿（Draft）** 版本的发布
3. **重要**：**不要点击"Publish release"** - 保持草稿状态
4. 从草稿中下载以下文件之一：
   - `aseprite-*-linux-x64.tar.xz` - 通用Linux归档（推荐）
   - `aseprite_*.deb` - Debian/Ubuntu软件包
   - `aseprite-bin-*.pkg.tar.zst` - Arch Linux软件包

   **架构限制**：所有构建产物仅支持 **amd64/x86_64** 架构，不支持ARM设备。

#### 5.2 文件格式说明

**`.tar.xz`归档** （推荐所有用户）：
- **优点**：
  - 通用格式，适用于所有Linux发行版
  - 无需root权限即可运行
  - 便携式，可放在任意目录
  - 包含所有运行时数据文件
  - 不修改系统文件
- **适用系统**：任何Linux发行版（Debian、Ubuntu、Arch、Fedora、openSUSE等）
- **桌面环境支持**：
  - **完全支持**：GNOME、KDE Plasma、XFCE、MATE、LXDE/LXQt、Cinnamon、Budgie等
  - **Wayland桌面**：通过XWayland支持（需安装`xorg-xwayland`）

**安装方式**：
```bash
# 解压到当前目录
tar -xJf aseprite-*.tar.xz

# 运行Aseprite
./aseprite-*/aseprite
```

**`.deb`包** （Debian/Ubuntu用户）：
- **优点**：
  - 自动安装到系统目录（`/usr/bin/aseprite`）
  - 创建桌面快捷方式和菜单项
  - 自动解决依赖关系
  - 可通过`apt`管理
- **适用系统**：Debian 13、Ubuntu 22.04+ 及其衍生发行版
- **桌面环境支持**：
  - **完全支持**：GNOME、KDE Plasma、XFCE、MATE、LXDE/LXQt、Cinnamon、Budgie等
  - **Wayland桌面**：通过XWayland支持（需安装`xorg-xwayland`）

**安装方式**：
```bash
sudo apt install ./aseprite_*.deb
```

**`.pkg.tar.zst`包** （Arch Linux用户）：
- **优点**：
  - 原生Arch Linux包格式
  - 自动安装到系统目录
  - 创建桌面快捷方式和菜单项
  - 可通过pacman管理
- **适用系统**：Arch Linux及其衍生发行版（Manjaro、Endeavor OS等）
- **桌面环境支持**：
  - **完全支持**：GNOME、KDE Plasma、XFCE、MATE、LXDE/LXQt、Cinnamon、Budgie等
  - **Wayland桌面**：通过XWayland支持（需安装`xorg-xwayland`）

**安装方式**：
```bash
# 使用pacman
sudo pacman -U ./aseprite-bin-*.pkg.tar.zst

# 或使用AUR助手（paru/yay等）
paru -U ./aseprite-bin-*.pkg.tar.zst
```

## 项目概述

这是一个自动化构建工具，用于在Linux环境中构建[Aseprite](https://www.aseprite.org/)像素艺术编辑器，并发布多种Linux安装格式。项目通过GitHub Actions工作流自动从上游Aseprite仓库获取最新版本源代码，编译并打包为`.tar.xz`归档、`.deb`软件包和Arch Linux的`.pkg.tar.zst`软件包。

## 功能特性

- **安全构建**：构建产物自动保持草稿状态，避免意外公开发布
- **自动构建**：自动检测Aseprite最新版本并下载源代码
- **依赖管理**：自动下载并配置Skia图形库（`aseprite-m124`分支）
- **多格式打包**：生成`.tar.xz`、`.deb`和Arch Linux可用的`.pkg.tar.zst`包
- **桌面集成**：自动创建桌面快捷方式、图标和应用程序菜单项
- **GitHub Actions集成**：支持标签触发和手动触发构建

## GitHub Actions工作流

### 触发方式

1. **手动触发**：通过GitHub Actions界面手动触发工作流（主要使用方式）
2. **标签触发**：当仓库中有新标签推送时自动运行

### 工作流输入参数

手动触发时，只需配置以下主要参数：

| 参数 | 描述 | 格式示例 | 默认值 |
|------|------|--------|--------|
| `aseprite_tag` | 上游Aseprite标签版本 | `v1.3.2`、`v1.3.1` 等 | 空值（自动使用最新版本） |

**其他参数（保留默认值即可）**：

| 参数 | 描述 | 默认值 |
|------|------|--------|
| `package_tarball` | 是否构建 tar.xz 归档 | `true` |
| `package_deb` | 是否构建 .deb 包 | `true` |
| `package_arch` | 是否构建 Arch Linux 包 | `true` |

**参数说明**：
- 绝大多数场景下，只需配置 `aseprite_tag` 参数
- 留空 `aseprite_tag` 将自动构建上游最新版本
- 其他参数采用默认值即可满足常规需求
- 仅当需要构建特定版本或自定义package信息时才修改其他参数

### 构建过程

工作流执行以下步骤：

1. **解析上游元数据**：获取Aseprite和Skia的最新版本信息
2. **下载源代码**：下载Aseprite源代码和预编译的Skia库
3. **构建Aseprite**：使用CMake和Ninja编译Aseprite
4. **Debian打包**：创建`.tar.xz`归档文件和`.deb`软件包
5. **Arch打包**：生成`aseprite-bin`格式的`.pkg.tar.zst`包
   - Arch打包为二进制重打包流程，`makepkg`使用`--nodeps`避免在CI容器中校验运行时依赖是否已安装
6. **草稿发布**：汇总所有构建产物并以草稿形式上传到GitHub Releases

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


## 常见问题与故障排查

### Q: 如何选择安装哪个文件格式？

**A:** 根据你的Linux发行版选择：

| 系统 | 推荐 | 备选 |
|------|------|------|
| Debian 13、Ubuntu | `.deb` | `.tar.xz` |
| Arch Linux、Manjaro | `.pkg.tar.zst` | `.tar.xz` |
| 其他发行版 | `.tar.xz` | - |
| 便携式使用或不确定 | `.tar.xz` | - |

### Q: 构建失败了怎么办？

**A:** 
1. 检查GitHub Actions日志中的错误信息
2. 确保fork仓库时GitHub Actions已启用
3. 查看是否有依赖解析或编译错误
4. 在GitHub Issues中报告问题

### Q: Wayland桌面无法启动Aseprite？

**A:** Aseprite依赖X11，在Wayland上运行需要XWayland支持：
```bash
# 安装XWayland
sudo apt install xorg-xwayland          # Debian/Ubuntu
sudo pacman -S xorg-xwayland            # Arch

# 然后在终端运行测试
aseprite --version
```

### Q: 如何查看构建产物的版本信息？

**A:**
```bash
# 查看binary版本
./aseprite --version

# 查看deb包信息
dpkg -I ./aseprite_*.deb

# 查看Arch包信息
pacman -Qip ./aseprite-bin-*.pkg.tar.zst
```

### Q: 如何自动化构建（例如每周）？

**A:** 修改`.github/workflows/aseprite-build-deploy.yml`，在`on:`部分添加schedule触发器：
```yaml
on:
  schedule:
    - cron: '0 0 * * 0'  # 每周日0点运行
  workflow_dispatch:
```

## 相关链接

- [Aseprite官方网站](https://www.aseprite.org/)
- [Aseprite GitHub仓库](https://github.com/aseprite/aseprite)
- [Skia图形库](https://github.com/aseprite/skia)
- [GitHub Actions文档](https://docs.github.com/en/actions)
- [Aseprite EULA](https://github.com/aseprite/aseprite/blob/main/EULA.txt)

---

*最后更新: 2026-06-04*
*重要提示：请严格遵守上述使用限制，尊重软件版权和许可条款*
