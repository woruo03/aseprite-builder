# Aseprite Builder for Linux

[![Build Aseprite for Linux](https://github.com/woruo03/aseprite-builder/actions/workflows/aseprite-build-deploy.yml/badge.svg)](https://github.com/woruo03/aseprite-builder/actions/workflows/aseprite-build-deploy.yml)

## 项目概述

在Linux上自动构建[Aseprite](https://www.aseprite.org/)像素艺术编辑器，生成多种安装格式（tar.xz、deb、rpm、pkg.tar.zst）。通过GitHub Actions自动从上游获取最新源代码，编译并打包，无需本地构建。

## ⚠️ 重要法律声明

**本项目仅供个人使用**。为尊重Aseprite的商业许可：

- **禁止公开发布**：构建的二进制文件只能保持为GitHub Releases草稿，不可发布
- **仅限个人使用**：不得分发、共享或用于商业目的
- **必须Fork**：用户需Fork到自己账户后再构建
- **遵守EULA**：使用前必读[Aseprite EULA](https://github.com/aseprite/aseprite/blob/main/EULA.txt)

本项目代码遵循MIT许可证。

## 快速开始

```
1. Fork 本仓库到你的账户
2. 进入 Actions → Build Aseprite for Linux
3. 点击 Run workflow → Run workflow
4. 等待完成后在 Releases 下载草稿中的产物
```

完成！构建自动使用最新Aseprite版本。

## 安装与使用

构建完成后在GitHub Releases下载以下文件之一：

| 文件格式                      | 适用系统      | 优点                   |
| ----------------------------- | ------------- | ---------------------- |
| `aseprite-*-linux-x64.tar.xz` | 所有Linux     | 无需root，便携式，通用 |
| `aseprite_*.deb`              | Debian/Ubuntu | 系统集成，自动依赖     |
| `aseprite-*.x86_64.rpm`       | Fedora/RHEL   | RPM原生格式            |
| `aseprite-bin-*.pkg.tar.zst`  | Arch Linux    | Arch原生格式           |

**注意**：仅支持**amd64 (x86_64)** 架构。

### 安装方式

**tar.xz归档**：

```bash
tar -xJf aseprite-*.tar.xz
./aseprite-*/aseprite
```

**deb包**（Debian/Ubuntu）：

```bash
sudo dpkg -i aseprite_*.deb
sudo apt-get install -f   # 若有缺失依赖则自动补全
aseprite
```

**rpm包**（Fedora/RHEL）：

```bash
sudo dnf install ./aseprite-*.x86_64.rpm
aseprite
```

**pkg.tar.zst包**（Arch）：

```bash
sudo pacman -U aseprite-bin-*.pkg.tar.zst
aseprite
```

### 桌面环境支持

- **X11**：完全支持（GNOME、KDE、XFCE、MATE等）
- **Wayland**：需XWayland支持
  ```bash
  sudo apt install xorg-xwayland        # Debian/Ubuntu
  sudo dnf install xorg-x11-server-Xwayland  # Fedora/RHEL
  sudo pacman -S xorg-xwayland          # Arch
  ```

## 功能特性

- 自动检测Aseprite最新版本（无需手动指定）
- Skia图形库使用预编译的`m124`分支版本
- 使用Clang + libstdc++工具链编译
- 多格式打包（tar.xz、deb、rpm、pkg.tar.zst）
- deb包依赖通过`dpkg-shlibdeps`动态解析
- rpm在`fedora:latest`容器中构建，Arch包在`archlinux:base-devel`容器中构建
- 桌面快捷方式和菜单集成（Desktop Entry + 图标）
- 草稿发布防止意外公开

## GitHub Actions工作流

### 触发方式

- **手动触发**：Actions界面点击Run workflow，可按需勾选打包格式
- **标签触发**：推送任意标签时自动运行，并构建全部四种格式

### 工作流参数（仅手动触发时生效）

| 参数              | 说明                                              | 默认值 |
| ----------------- | ------------------------------------------------- | ------ |
| `aseprite_tag`    | Aseprite版本（如v1.3.2，留空使用上游最新release） | 空     |
| `package_tarball` | 是否构建tar.xz                                    | true   |
| `package_deb`     | 是否构建deb包                                     | true   |
| `package_arch`    | 是否构建Arch包                                    | true   |
| `package_rpm`     | 是否构建RPM包                                     | true   |

### 构建流程

工作流由6个并行/串行的job组成：

1. **build-aseprite**：解析上游Aseprite与Skia(`m124`)版本元数据，下载源码和预编译Skia库，使用CMake + Ninja + Clang(libstdc++)完成编译，再 strip + 复制 `data/`目录生成发布目录
2. **package-tarball**：将发布目录打包为`aseprite-<tag>-linux-x64.tar.xz`
3. **package-deb**：使用`dpkg-shlibdeps`解析运行时依赖并通过`dpkg-deb`生成`.deb`
4. **package-arch**（在`archlinux:base-devel`容器中）：生成`PKGBUILD`并执行`makepkg`产出`.pkg.tar.zst`
5. **package-rpm**（在`fedora:latest`容器中）：生成`.spec`并执行`rpmbuild`产出`.rpm`
6. **publish-draft-release**：汇总所有构建产物，创建GitHub Releases草稿（不公开、非latest）

### 发布命名

- Release tag: `aseprite-<aseprite_tag>-linux-x64`
- Release name: `Aseprite <aseprite_tag> for Linux (x64)`
- 始终以草稿形式创建，不会自动发布或标记为latest

## 汉化指南（简体中文）

Aseprite提供官方认可的社区翻译。

### 安装步骤

**1. 获取汉化文件**

从[Weblate翻译平台](https://hosted.weblate.org/projects/aseprite/-/zh_Hans/)下载zip压缩包，解压出`zh_Hans.ini`文件。

**2. 创建扩展目录**

```bash
mkdir -p ~/.config/aseprite/extensions/zh-hans
cp ~/Downloads/zh_Hans.ini ~/.config/aseprite/extensions/zh-hans/
```

**3. 创建清单文件**

```bash
cat > ~/.config/aseprite/extensions/zh-hans/package.json << 'EOF'
{
  "name": "zh-hans",
  "version": "1.0",
  "displayName": "简体中文",
  "categories": [ "Languages" ],
  "contributes": {
    "languages": [
      { "id": "zh-Hans", "path": "./zh_Hans.ini" }
    ]
  }
}
EOF
```

**4. 在Aseprite中启用**

1. 打开Aseprite
2. 按`Ctrl + K`打开首选项
3. 选择**General** → **Language** → **简体中文**
4. 点击**Apply**后重启

### 优点

- ✅ 用户级：无需root权限
- ✅ 可维护：系统更新不影响
- ✅ 易卸载：删除目录即可
- ✅ 标准方式：符合Aseprite扩展规范

## 常见问题

### Q: 如何选择安装格式？

| 系统          | 推荐        | 备选   |
| ------------- | ----------- | ------ |
| Debian/Ubuntu | deb         | tar.xz |
| Fedora/RHEL   | rpm         | tar.xz |
| Arch          | pkg.tar.zst | tar.xz |
| 其他/便携式   | tar.xz      | -      |

### Q: 构建失败怎么办？

1. 检查GitHub Actions日志中的错误信息
2. 确保fork时GitHub Actions已启用
3. 查看依赖解析或编译错误
4. 在Issues中报告问题

### Q: Wayland无法启动Aseprite？

安装XWayland后在终端运行测试：

```bash
sudo apt install xorg-xwayland                   # Debian/Ubuntu
sudo dnf install xorg-x11-server-Xwayland         # Fedora/RHEL
sudo pacman -S xorg-xwayland                      # Arch
aseprite --version
```

### Q: 如何查看版本信息？

```bash
./aseprite --version                     # 二进制版本
dpkg -I ./aseprite_*.deb                 # deb包信息
rpm -qip ./aseprite-*.x86_64.rpm         # RPM包信息
pacman -Qip ./aseprite-bin-*.pkg.tar.zst # Arch包信息
```

### Q: 如何自动化构建（如每周）？

修改`.github/workflows/aseprite-build-deploy.yml`，在`on:`下增加`schedule`：

```yaml
on:
  push:
    tags:
      - "*"
  schedule:
    - cron: "0 0 * * 0" # 每周日0点
  workflow_dispatch:
```

注意：`schedule`触发时不会传入`workflow_dispatch`的inputs，工作流会按默认行为构建全部四种格式。

## 相关链接

- [Aseprite官方网站](https://www.aseprite.org/)
- [Aseprite GitHub仓库](https://github.com/aseprite/aseprite)
- [Skia图形库](https://github.com/aseprite/skia)
- [GitHub Actions文档](https://docs.github.com/en/actions)
- [Aseprite EULA](https://github.com/aseprite/aseprite/blob/main/EULA.txt)

---

_最后更新: 2026-06-13_
_请严格遵守使用限制，尊重软件版权和许可条款_
