# FFmpeg aarch64 交叉编译（GitHub Actions）

## 使用步骤

### 第一步：在 GitHub 创建一个新仓库

1. 打开 https://github.com/new
2. 仓库名随意，例如 `ffmpeg-aarch64-build`
3. 选 **Public** 或 **Private** 均可
4. 点击 "Create repository"

---

### 第二步：把这个目录推到 GitHub

打开你的终端（Git Bash），执行以下命令：

```bash
cd /path/to/ffmpeg-aarch64   # 进入本项目目录

git init
git add .
git commit -m "init: add FFmpeg aarch64 cross-compile workflow"
git branch -M main

# 替换 YOUR_USERNAME 和 YOUR_REPO 为你实际的用户名和仓库名
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

---

### 第三步：等待 GitHub Actions 自动编译

push 之后，打开仓库页面：

1. 点击顶部的 **Actions** 标签
2. 可以看到 `Build FFmpeg aarch64 (Linaro 7.5.0)` 正在运行
3. 编译耗时约 **15～30 分钟**

---

### 第四步：下载编译产物

编译成功后：

1. 点击那次 workflow 运行记录
2. 页面底部 **Artifacts** 区域
3. 下载 `ffmpeg-aarch64-linaro750.zip`
4. 解压后可以看到：

```
output/
  ├── lib/
  │   ├── libavcodec.so.61
  │   ├── libavformat.so.61
  │   ├── libavutil.so.59
  │   ├── libswscale.so.8
  │   ├── libswresample.so.5
  │   └── libavfilter.so.10
  ├── include/
  │   ├── libavcodec/
  │   ├── libavformat/
  │   └── ...
  └── bin/
      ├── ffmpeg
      └── ffprobe
```

---

### 也可以手动触发（不需要 push）

1. 打开仓库 → **Actions** → 左侧选 `Build FFmpeg aarch64`
2. 点右侧 **Run workflow** 按钮
3. 点绿色 **Run workflow** 确认

---

## 编译参数说明

| 参数 | 说明 |
|------|------|
| `--enable-cross-compile` | 启用交叉编译模式 |
| `--cross-prefix=aarch64-linux-gnu-` | 使用 Linaro 工具链前缀 |
| `--arch=aarch64` | 目标架构 ARM64 |
| `--enable-shared` | 生成 `.so` 动态库 |
| `--disable-static` | 不生成 `.a` 静态库 |
| `--disable-ffplay` | 不编译播放器（嵌入式不需要） |
| `--enable-gpl` | 启用 GPL 组件 |
| `--extra-cflags="-march=armv8-a -O2"` | ARMv8-A 优化指令集 |

---

## 如需添加第三方编解码器（x264、opus 等）

需要先交叉编译对应的库，然后在 configure 中添加：

```
--enable-libx264
--enable-libopus
--enable-libvorbis
```

有需要可以告知，我来扩展 workflow。
