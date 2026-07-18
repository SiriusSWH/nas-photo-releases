# Docker 通用安装指南

适用于任何能跑 Docker 的 x86_64 NAS / 服务器：飞牛（不想用原生版时）、威联通 Container Station、铁威马、极空间、绿联、Unraid、TrueNAS、普通 Linux 主机等。

镜像：**`ghcr.io/siriusswh/nas-photo:allinone`**（公开，无需登录；也可用固定版本 tag 如 `:0.1.14`）

## 方式一：docker run

```bash
docker run -d --name nas-photo \
  --restart unless-stopped \
  -p 8080:8080 \
  -v /你的路径/nas-photo-data:/data \
  -v /你的照片目录:/photo \
  -e LIBRARY_ROOT_WHITELIST=/photo \
  ghcr.io/siriusswh/nas-photo:allinone
```

## 方式二：docker compose

```yaml
services:
  app:
    image: ghcr.io/siriusswh/nas-photo:allinone
    container_name: nas-photo
    ports:
      - "8080:8080"
    volumes:
      - /你的路径/nas-photo-data:/data   # 数据库 + 缩略图 + AI 模型
      - /你的照片目录:/photo
    environment:
      LIBRARY_ROOT_WHITELIST: /photo
    restart: unless-stopped
```

## 方式三：NAS 的 Docker 图形界面

以飞牛为例（威联通 / 极空间 / 绿联同理）：

1. Docker → 镜像 → 从 URL 拉取：`ghcr.io/siriusswh/nas-photo:allinone`
2. 创建容器，按下表配置：

| 配置 | 值 |
|---|---|
| 端口 | 宿主 `8080` → 容器 `8080` |
| 挂载① 数据 | NAS 上的数据目录 → 容器 `/data` |
| 挂载② 照片 | 你的照片目录 → 容器 `/photo` |
| 环境变量 | `LIBRARY_ROOT_WHITELIST` = `/photo` |

> `/data` 建议预留 ≥ 10GB（数据库 + 缩略图 +（若开 AI）约 3.4GB 模型）。

## 开始使用

1. 浏览器访问 `http://<设备IP>:8080` → **注册新账号**（第一个账号即管理员）
2. **设置 → 图库管理 → 添加图库** → 路径浏览器选 `/photo`（**容器内路径**）→ 保存 → **扫描**
3. 需要 AI（人脸 / 自然语言搜图 / 文字搜索）：**设置 → AI** → 一键下载（约 3.4GB 到 `/data`，容器重建不丢）

> ⚠️ 应用内「永久删除」会真实删除原文件。第一次建议先挂测试子目录验证。

## 升级

拉取新镜像 → 删除旧容器 → 用同样参数重建。`/data` 卷不动，数据全保留。

```bash
docker pull ghcr.io/siriusswh/nas-photo:allinone
docker rm -f nas-photo
# 重新执行上面的 docker run
```

## 常见问题

| 现象 | 处理 |
|---|---|
| 8080 打不开 | 检查设备防火墙；`docker logs nas-photo` 看容器是否正常启动 |
| 添加图库看不到目录 | 图库路径填**容器内**的 `/photo`；确认挂载和 `LIBRARY_ROOT_WHITELIST` 一致 |
| 想改对外端口 | 只改宿主侧（如 `-p 18080:8080`），容器内 8080 不变 |
| ARM 设备 | 暂不支持，目前仅 x86_64 |
