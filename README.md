# Aseprite Builder for Linux

[![Build Aseprite for Linux](https://github.com/woruo03/aseprite-builder/actions/workflows/aseprite-build-deploy.yml/badge.svg)](https://github.com/woruo03/aseprite-builder/actions/workflows/aseprite-build-deploy.yml)

## 项目概述

在Linux上自动构建[Aseprite](https://www.aseprite.org/)像素艺术编辑器，生成多种安装格式（tar.xz、deb、pkg.tar.zst）。通过GitHub Actions自动从上游获取最新源代码，编译并打包，无需本地构建。

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

| 文件格式 | 适用系统 | 优点 |
|---------|---------|------|
| `aseprite-*-linux-x64.tar.xz` | 所有Linux | 无需root，便携式，通用 |
| `aseprite_*.deb` | Debian/Ubuntu | 系统集成，自动依赖 |
| `aseprite-bin-*.pkg.tar.zst` | Arch Linux | Arch原生格式 |

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
  sudo apt install xorg-xwayland    # Debian/Ubuntu
  sudo pacman -S xorg-xwayland      # Arch
  ```

## 功能特性

- 自动检测Aseprite最新版本
- 多格式打包（tar.xz、deb、pkg.tar.zst）
- 自动配置Skia图形库
- 桌面快捷方式和菜单集成
- 草稿发布防止意外公开

## GitHub Actions工作流

### 触发方式

- **手动触发**：Actions界面点击Run workflow
- **标签触发**：推送标签时自动运行

### 工作流参数

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `aseprite_tag` | Aseprite版本（如v1.3.2，留空用最新） | 空 |
| `package_tarball` | 是否构建tar.xz | true |
| `package_deb` | 是否构建deb包 | true |
| `package_arch` | 是否构建Arch包 | true |

### 构建流程

1. 解析Aseprite和Skia版本信息
2. 下载源代码和预编译Skia库
3. 使用CMake和Ninja编译
4. 生成多格式安装包
5. 上传为GitHub Releases草稿

## 汉化指南（简体中文）

Aseprite提供官方认可的社区翻译。

### 安装步骤

**1. 获取汉化文件**

从[Weblate翻译平台](https://hosted.weblate.org/projects/aseprite/-/zh_Hans/)下载zip压缩包，解压`zh_Hans.ini`文件。

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

| 系统 | 推荐 | 备选 |
|------|------|------|
| Debian/Ubuntu | deb | tar.xz |
| Arch | pkg.tar.zst | tar.xz |
| 其他/便携式 | tar.xz | - |

### Q: 构建失败怎么办？

1. 检查GitHub Actions日志中的错误信息
2. 确保fork时GitHub Actions已启用
3. 查看依赖解析或编译错误
4. 在Issues中报告问题

### Q: Wayland无法启动Aseprite？

安装XWayland后在终端运行测试：
```bash
sudo apt install xorg-xwayland       # Debian/Ubuntu
sudo pacman -S xorg-xwayland         # Arch
aseprite --version
```

### Q: 如何查看版本信息？

```bash
./aseprite --version                # 二进制版本
dpkg -I ./aseprite_*.deb            # deb包信息
pacman -Qip ./aseprite-bin-*.pkg.tar.zst  # Arch包信息
```

### Q: 如何自动化构建（如每周）？

修改`.github/workflows/aseprite-build-deploy.yml`：
```yaml
on:
  schedule:
    - cron: '0 0 * * 0'  # 每周日0点
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
*请严格遵守使用限制，尊重软件版权和许可条款*
