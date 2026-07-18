<p align="center">
  <img src="assets/icon-256.png" width="128" alt="NAS Photo" />
</p>

<h1 align="center">NAS Photo</h1>

<p align="center">部署在你自己 NAS 上的照片管理服务 —— 数据 100% 在自己手里。<br>
时间线 · 相册 · Live Photo · RAW · AI 人脸聚类 · 中文自然语言搜图 · 照片文字搜索</p>

---

## 特性

- **时间线**：按月分组、缩略图渐进加载、6 档缩放、排序筛选、多选批量操作
- **相册 / 收藏 / 回收站**：自建相册、封面、排序策略；软删除可恢复
- **Live Photo**：HEIC+MOV 自动配对，点按播放实况
- **RAW**：21 种主流相机 RAW 格式（ARW/CR3/NEF/DNG…），RAW+JPEG 自动配对，完整 EXIF
- **视频**：流式播放、封面抽帧、拍摄时间正确解析（含 iPhone 时区处理）
- **详情页**：完整 EXIF、GPS 反向地理编码 + 地图、胶片条快速切换
- **AI（可选，一键开启）**：
  - 人脸识别与聚类 —— 按人物浏览
  - 自然语言搜图 —— 中文直接搜「海边的狗」
  - 照片文字识别（OCR）—— 搜截图和文档照片里的文字
- **隐私**：所有数据（照片、数据库、AI 模型、AI 推理）都在你的 NAS 本地，不上传任何内容

## 安装

| 平台 | 方式 | 文档 |
|---|---|---|
| **飞牛 fnOS** | 原生应用（.fpk，桌面图标，无需 Docker）· 推荐 | [安装指南](docs/install-fnos.md) |
| **群晖 Synology** | Container Manager（Docker） | [安装指南](docs/install-synology.md) |
| **其它任意带 Docker 的 NAS**（威联通 / 铁威马 / 极空间 / 绿联 / Unraid / TrueNAS…） | Docker | [安装指南](docs/install-docker.md) |

Docker 镜像：`ghcr.io/siriusswh/nas-photo:allinone`（x86_64，公开无需登录）

## 系统要求

- x86_64 架构 NAS（arm 暂不支持）
- 基础功能：镜像约 400MB，内存占用低
- AI 功能（可选）：首次在「设置 → AI」一键下载约 3.4GB 到数据盘（国内自动走镜像源），4GB+ 内存建议

## 第一次使用

1. 打开 Web 界面 → **注册新账号**（第一个账号即管理员）
2. 设置 → 图库管理 → 添加图库 → 选择照片目录 → 扫描
3. 扫描完成后时间线出图；需要 AI 就在 设置 → AI 一键开启

> ⚠️ 应用内「永久删除」会真实删除原文件。第一次建议先用测试目录验证，确认符合预期再接入整个照片库。

## 版本与更新

- 当前版本见 [Releases](../../releases)，每个版本附更新说明（[CHANGELOG](CHANGELOG.md)）
- 飞牛原生版：应用中心手动安装新 .fpk 即可升级，数据保留
- Docker 版：拉取新镜像重建容器即可，`/data` 数据卷不动

## 反馈

- Bug / 需求：[Issues](../../issues)
- 讨论 / 求助：[Discussions](../../discussions)

## 许可

本软件**免费使用**，闭源发行。详见 [EULA](EULA.md)。

---

## English

NAS Photo is a self-hosted photo management service for your NAS: timeline, albums, Live Photos, RAW (21 formats), video, face clustering, natural-language photo search (Chinese & English), and OCR text search — all running 100% locally on your hardware, nothing leaves your network.

Install via Docker: `ghcr.io/siriusswh/nas-photo:allinone` (x86_64). See [docs/install-docker.md](docs/install-docker.md). Free to use, closed source ([EULA](EULA.md)).
