# Archived due to docker.io `429`

---

# Cloudflare Registry Proxy

![License](https://img.shields.io/badge/license-MIT-green)

基于Cloudflare Workers的Docker镜像仓库代理服务，主要用于个人或小型团队环境下加速Docker镜像拉取。

## 📑 功能特性

- **自用优先**: 专为个人开发环境或小型团队设计，部署简单，无需复杂配置
- **镜像白名单**: 支持配置允许下载的Docker镜像白名单，提升安全性
- **透明代理**: 无缝代理Docker Hub及其他镜像仓库的请求
- **智能缓存**: 利用Cloudflare的缓存能力加速镜像拉取
- **零成本部署**: 利用Cloudflare Workers免费额度，个人使用基本不产生费用

## 🚀 快速开始

### 前提条件

- Cloudflare账号
- Node.js 16+
- npm or yarn

### 安装部署

1. 克隆项目

```bash
git clone https://github.com/yourusername/cloudflare-registry-proxy.git
cd cloudflare-registry-proxy
```

2. 安装依赖

```bash
npm install
```

3. 配置白名单（可选）

编辑`wrangler.jsonc`文件，配置允许下载的镜像白名单：

```jsonc
"vars": {
  "WHITELIST": ["library/nginx", "subfuzion/netcat", "library/*"]
}
```

> 白名单支持通配符，例如: `"library/*"` 表示允许所有官方镜像。如不配置，将默认使用`["library/nginx", "subfuzion/netcat"]`作为白名单。

4. 本地开发测试

```bash
npm run dev
```

5. 部署到Cloudflare Workers

```bash
npm run deploy
```

## 🔒 白名单功能详解

白名单功能是本项目的核心安全特性，可以限制允许通过代理拉取的Docker镜像，防止恶意使用。

### 白名单配置格式

白名单配置为字符串数组，每个字符串代表一个允许下载的镜像名称模式：

```json
["library/nginx", "subfuzion/netcat", "library/*"]
```

### 支持的匹配模式

- **精确匹配**: 如`library/nginx`，只允许下载完全匹配的镜像
- **前缀匹配**: 如`library/*`，允许下载所有`library`组织的镜像
- **无白名单**: 如果不设置白名单，默认使用`["library/nginx", "subfuzion/netcat"]`

### 配置示例

常见场景的白名单配置：

```jsonc
// 只允许官方nginx和redis镜像
"WHITELIST": ["library/nginx", "library/redis"]

// 允许所有官方镜像
"WHITELIST": ["library/*"]

// 允许特定组织的所有镜像
"WHITELIST": ["yourorg/*"]
```

## 🔍 如何使用

部署完成后，可以通过以下方式使用代理：

### 直接使用

```bash
docker pull your-worker-subdomain.workers.dev/v2/library/nginx
```

### 配置Docker客户端

在Docker配置中添加镜像仓库：

```json
{
  "registry-mirrors": [
    "https://your-worker-subdomain.workers.dev"
  ]
}
```

## 🛠️ 技术实现

- 基于Cloudflare Workers平台
- 无需服务器，零运维成本
- 利用Cloudflare全球边缘网络加速镜像拉取

## 📝 注意事项

- 此项目主要面向个人开发环境或小型团队使用
- 大规模生产环境建议使用专业的镜像仓库服务
- 免费账户有一定的使用限制，请参考Cloudflare Workers的[使用限制](https://developers.cloudflare.com/workers/platform/limits/)

## 🤝 贡献指南

欢迎提交问题和功能建议！如果想要贡献代码，请Fork本仓库并创建Pull Request。

## 📄 许可证


本项目采用MIT许可证 - 详情请查看[LICENSE](LICENSE)文件。
