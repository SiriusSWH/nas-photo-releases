# 群晖 Synology 安装指南（Container Manager）

适用于 DSM 7.2+ 的 x86_64 机型（Plus 系列等）。全程图形界面，约 10 分钟。

## 1. 准备目录

打开 **File Station**，在 `docker` 共享文件夹下新建：

```
docker/nas-photo/data      ← 应用数据（数据库、缩略图、AI 模型）
```

照片目录用你现有的（例如共享文件夹 `photo`）。

## 2. 创建项目

1. 打开 **Container Manager**（旧版叫 Docker，套件中心可安装）
2. 左侧 **项目 → 新增**：
   - 项目名称：`nas-photo`
   - 路径：选择 `/docker/nas-photo`
   - 来源：**创建 docker-compose.yml**，粘贴：

```yaml
services:
  app:
    image: ghcr.io/siriusswh/nas-photo:allinone
    container_name: nas-photo
    ports:
      - "8080:8080"
    volumes:
      - /volume1/docker/nas-photo/data:/data
      - /volume1/photo:/photo        # 改成你的照片目录
    environment:
      LIBRARY_ROOT_WHITELIST: /photo
    restart: unless-stopped
```

> 两处按需修改：`/volume1/photo` 换成你的照片实际路径；若 8080 已被占用，把左边改成其它端口（如 `18080:8080`）。

3. 下一步 → 完成。首次会拉取镜像（约 400MB，公开镜像无需登录）。

## 3. 开始使用

1. 浏览器访问 `http://<群晖IP>:8080` → **注册新账号**（第一个账号即管理员）
2. **设置 → 图库管理 → 添加图库** → 路径浏览器选 `/photo`（**容器内路径**，不是 /volume1/...）
3. 保存 → **扫描**，完成后时间线出图

> ⚠️ 应用内「永久删除」会真实删除原文件。第一次建议先挂一个测试子目录，确认符合预期再挂整库。

## 4. 开启 AI（可选）

**设置 → AI** → 一键下载（约 3.4GB 到 `data` 目录，国内自动走镜像源）。
完成后可用：人脸聚类、中文自然语言搜图、照片文字搜索。

## 5. 升级

Container Manager → **映像** → 找到 `ghcr.io/siriusswh/nas-photo` → 更新（拉新镜像）→ 项目里**重新构建**。
`data` 目录不动，账号、索引、AI 模型全部保留。

## 常见问题

| 现象 | 处理 |
|---|---|
| 8080 打不开 | 控制面板 → 安全性 → 防火墙，放行 8080；确认容器状态为运行中 |
| 添加图库看不到目录 | 图库路径填**容器内**的 `/photo`；确认 compose 里 volumes 挂载正确 |
| 拉取镜像慢 | 网络问题，可重试；镜像托管在 GitHub（ghcr.io） |
| ARM 机型（j 系列等） | 暂不支持，目前仅 x86_64 |
