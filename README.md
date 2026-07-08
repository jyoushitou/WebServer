# WebServer - C++ HTTP/2 RPC 微服务框架

> **从零实现的 C++ HTTP 服务器 → HTTP/2 RPC 微服务架构演进**
>
> 用最底层的方式理解 Web 工作原理，用微服务架构承载业务扩展

![C++](https://img.shields.io/badge/C++-17-%2300599C?style=flat-square&logo=c%2B%2B)
![Rust](https://img.shields.io/badge/Rust-1.70-%23DEA584?style=flat-square&logo=rust)
![Go](https://img.shields.io/badge/Go-1.21-%2300ADD8?style=flat-square&logo=go)
![FFmpeg](https://img.shields.io/badge/FFmpeg-6.0-%23008080?style=flat-square&logo=ffmpeg)
![Vue](https://img.shields.io/badge/Vue-3-%234FC08D?style=flat-square&logo=vue.js)
![Protobuf](https://img.shields.io/badge/Protobuf-3.15-%23FF6C37?style=flat-square&logo=protocol-buffers)
![Elasticsearch](https://img.shields.io/badge/Elasticsearch-7-%23005571?style=flat-square&logo=elasticsearch)
![MySQL](https://img.shields.io/badge/MySQL-8-%234479A1?style=flat-square&logo=mysql)
![Docker](https://img.shields.io/badge/Docker-24-%232496ED?style=flat-square&logo=docker)
![Kubernetes](https://img.shields.io/badge/Kubernetes-1.28-%23326CE5?style=flat-square&logo=kubernetes)
![mTLS](https://img.shields.io/badge/mTLS-1.3-%23FF4B4B?style=flat-square&logo=letsencrypt)
![AES-256](https://img.shields.io/badge/AES--256-GCM-%2300BFA0?style=flat-square&logo=security)
![Boost](https://img.shields.io/badge/Boost-1.83-%23F7901E?style=flat-square&logo=boost)

---

## 📖 项目简介

本项目是一个**从零开始、不依赖任何第三方 Web 框架**的 C++ HTTP 服务器，逐步演进为完整的 Protobuf 微服务架构。

> **原 C++ 单体后端已归档**：[WebServer_cpp](https://github.com/jyoushitou/WebServer_cpp.git)
>
> 归档版本为纯 C++ 实现的单体 HTTP 服务器，当前仓库为微服务架构演进版本。

### 架构演进路线

```
阶段一：单体架构（已归档）
  C++ 原生 Socket HTTP 服务器 + Vue 3 前端
  └── 手动解析 HTTP 协议、多线程处理、MySQL 直连
  └── 归档仓库：https://github.com/jyoushitou/WebSever_cpp.git

阶段二：微服务架构（进行中）
  手写 RPC 框架 + 独立 Proto 仓库 + 服务拆分
  │
  ├── 核心框架（C++ 实现）
  │   ├── RPCGateway         — 对外统一入口，协议转换/路由分发/限流/鉴权
  │   ├── ServiceRegistry     — 服务注册发现，动态路由/负载均衡/健康检查
  │   ├── ConfigCenter        — 配置中心，统一配置管理/热更新/版本控制
  │   ├── TracingService      — 链路追踪，请求全链路跟踪/性能分析/故障排查
  │   ├── MonitorService      — 监控告警，指标采集/告警规则/可视化面板
  │   ├── AdminConsole        — 内部管理面板（独立 Web UI）
  │   └── ServiceConsole      — 业务管理面板（独立 Web UI）
  │
  ├── 业务微服务（多语言）
  │   ├── C++（高性能计算密集型）
  │   │   ├── ImageService    — 图片像素级处理/缩略图生成/格式转换 (端口:50055)
  │   │   └── SearchService   — 全文索引/搜索排序/ES 集成 (端口:50057)
  │   ├── Go（I/O 密集型）
  │   │   ├── ArticleService  — 文章 CRUD/分类标签管理 (端口:50053)
  │   │   ├── BlogService     — 博客管理/评论系统/点赞收藏 (端口:50054)
  │   │   └── VideoService    — 视频上传/FFmpeg 转码调度/流媒体 (端口:50056)
  │   └── Rust（安全敏感）
  │       ├── UserService     — 用户注册登录/Token 管理/权限控制 (端口:50052)
  │       └── SecurityService — 数据加密解密/密钥管理/数字签名 (端口:51057) 🔐
  │
  ├── Proto 驱动开发
  │   ├── 所有跨语言通信接口由 proto 文件统一定义在 proto/source/ 目录下
  │   ├── 通过 protoc 的 --cpp_out / --go_out / --rust_out 编译生成各语言的序列化代码
  │   │   ├── protoc --cpp_out   → C++   → 核心框架 + ImageService + SearchService
  │   │   ├── protoc --go_out    → Go    → ArticleService + BlogService + VideoService
  │   │   └── protoc --rust_out  → Rust  → UserService + SecurityService
  │   └── 服务间通信使用自定义 TCP 帧协议 + Protobuf 序列化
  │
  └── 安全规划
      ├── SecurityService 加密安全微服务（Rust），提供数据加密、密钥管理、数字签名
      └── CertService 证书分发微服务（C++），提供 SSL/TLS 证书自动分发、续期、Nginx 配置生成

阶段三：容器化部署（规划中）
  Docker + Kubernetes + CI/CD
  ├── Docker 容器化，Docker Compose 本地编排
  ├── Kubernetes 集群部署，弹性伸缩
  ├── CI/CD 自动化流水线（GitHub Actions / GitLab CI）
  ├── 监控告警（Prometheus + Grafana）
  └── 链路追踪（Jaeger）

阶段四：安全加固体系（规划中）
  纵深防御 + 零信任架构
  ├── 应用层安全 — SecurityService 全链路加密微服务（Rust）
  │   ├── 敏感数据端到端加密（AES-256-GCM 字段级加密）
  │   ├── 密钥生命周期管理（自动轮换，30 天策略）
  │   └── 数字签名验签（Ed25519 / ECDSA）
  ├── 传输层安全
  │   ├── 服务间 mTLS 双向认证（所有 RPC 通信启用 TLS）
  │   ├── 证书自动轮换（CertService 统一 CA 签发，7 天轮换策略）
  │   ├── 网关和所有微服务均从 CertService 获取证书
  │   └── 网关统一 TLS termination
  ├── 存储层安全
  │   ├── 数据库 TDE（透明数据加密）
  │   ├── 文件存储加密（AES-256-CBC）
  │   └── 密钥硬安全模块（HSM）集成
  ├── 审计与合规
  │   ├── 审计日志系统（所有加解密操作可追溯）
  │   ├── 安全合规（GDPR / 等保 2.0 / SOC2 对标）
  │   └── 零信任架构落地（每次请求都鉴权加密）
  └── 持续安全
      ├── 依赖漏洞扫描（Dependabot / Trivy）
      ├── 代码安全审计（Rust 内存安全 + C++ 静态分析）
      └── 渗透测试与红蓝对抗
```

---

## 🏗️ 项目结构

```
WebServer/                              # 总仓库（Git 根仓库）
│
├── proto/                              # [子仓库] Proto 接口定义
│   ├── source/                         # Proto 源文件
│   │   ├── common/                     # 公共类型、错误码
│   │   ├── gateway/                    # 网关服务接口
│   │   ├── registry/                   # 服务注册发现接口
│   │   ├── config/                     # 配置中心接口
│   │   ├── tracing/                    # 链路追踪接口
│   │   ├── monitor/                    # 监控告警接口
│   │   ├── user/                       # 用户服务接口
│   │   ├── article/                    # 文章服务接口
│   │   ├── blog/                       # 博客服务接口
│   │   ├── image/                      # 图片服务接口
│   │   ├── video/                      # 视频服务接口
│   │   ├── search/                     # 搜索服务接口
│   │   ├── frontend/                   # 前端服务接口
│   │   ├── admin_console/              # 内部管理面板接口
│   │   └── service_console/            # 业务管理面板接口
│   └── build/                          # 编译输出
│
├── AdminConsole/                       # [子仓库] 内部管理面板 (C++) ✅ 核心
│   └── 统一管控核心基础设施（网关/注册发现/配置中心/链路追踪/监控告警）的独立 Web 控制台
│
├── ServiceConsole/                     # [子仓库] 业务服务管理面板 (C++) ✅ 核心
│   └── 统一管理业务数据（用户/文章/博客/图片/视频/搜索）的独立 Web 控制台
│
├── RPCGateway/                        # [子仓库] 网关 (C++) ✅ 核心
│   └── 对外统一入口，HTTP/Protobuf 协议转换、路由分发、限流熔断、身份鉴权
│
├── ServiceRegistry/                    # [子仓库] 服务注册发现 (C++) ✅ 核心
│   └── 动态服务注册表，提供服务发现、健康检测、负载均衡、上下线通知
│
├── ConfigCenter/                       # [子仓库] 配置中心 (C++) ✅ 核心
│   └── 集中管理所有微服务配置项，支持运行时热更新、版本回溯、变更推送
│
├── TracingService/                     # [子仓库] 链路追踪 (C++) ✅ 核心
│   └── 采集分布式请求调用链数据，可视化服务依赖拓扑，定位性能瓶颈
│
├── MonitorService/                     # [子仓库] 监控告警 (C++) ✅ 核心
│   └── 实时采集 CPU/内存/QPS/延迟等指标，配置告警规则，异常自动通知
│
├── UserService/                        # [子仓库] 用户服务 (Rust)
│   └── 用户注册/登录、Token 签发验证、RBAC 权限管理、多设备登录管理
│
├── SecurityService/                    # [子仓库] 加密安全服务 (Rust) 🔐
│   └── 数据加密解密（AES-256-GCM/RSA-ECIES）、密钥管理、数字签名
│
├── CertService/                        # [子仓库] 证书分发服务 (C++) 🔐
│   └── SSL/TLS 证书自动分发、自动续期、Nginx 配置生成、ACME 协议对接
│
├── ArticleService/                     # [子仓库] 文章服务 (Go)
│   └── 文章 CRUD、文章搜索、分类/标签管理，Go 高并发协程应对大量读写请求
│
├── BlogService/                        # [子仓库] 博客服务 (Go)
│   └── 博客发布、评论互动、点赞收藏、个性化推荐，goroutine 高效处理并发互动
│
├── ImageService/                       # [子仓库] 图片服务 (C++)
│   └── 图片上传、像素级处理、多尺寸缩略图、格式转换（JPEG/PNG/WebP/AVIF），C++ 极致利用 CPU 多核
│
├── VideoService/                       # [子仓库] 视频服务 (Go)
│   └── 视频上传、FFmpeg 转码任务编排调度、HLS 切片分发、流媒体播放
│
├── SearchService/                      # [子仓库] 搜索服务 (C++)
│   └── 全文检索、搜索排序评分、搜索建议、Elasticsearch 索引管理，C++ 保证毫秒级响应
│
├── vue/                                # [子仓库] Vue 3 前端
│   └── 用户端 Web 应用，Vue 3 + TypeScript，通过 HTTP/JSON 与网关通信
│
├── mobile/                             # [子仓库] 移动端 App
│   ├── ios/                            # iOS App (Swift) — 用户端，浏览文章/博客/图片/视频
│   └── android/                        # Android App (Kotlin) — 用户端，浏览文章/博客/图片/视频
│   └── 原生移动端应用，通过 HTTP/JSON 连接后端服务
│
├── desktop/                            # [子仓库] Qt 桌面端 App (C++)
│   └── Qt 6 + C++17 高性能桌面客户端，TCP/Protobuf 直连网关，支持 Windows/macOS/Linux
│
├── assets/                             # [子仓库] 设计资源
│   ├── designs/                        # UI 设计稿（Figma / Sketch）
│   ├── icons/                          # 图标库
│   ├── logos/                          # Logo 资源
│   └── mockups/                        # 产品原型 / 交互稿
│   └── 全平台设计资产仓库，统一管理前端和移动端的设计交付物
│
├── boost/                              # Boost 库（header-only，直接包含在仓库中）
│   ├── boost/                          # Boost 头文件（asio/beast/algorithm 等）
│   └── bin.v2/                         # Boost.Build 构建缓存
│   └── 所有 C++ 微服务（RPCGateway/ServiceRegistry/ConfigCenter/TracingService/MonitorService/AdminConsole/ServiceConsole/ImageService/SearchService/CertService）均依赖此 Boost 库
│   └── 核心依赖：Boost.Beast（HTTP/WebSocket）、Boost.Asio（网络 I/O）、Boost.JSON（JSON 解析）、Boost.Algorithm（算法扩展）
│
├── .gitmodules                         # Git 子模块配置
└── README.md                           # 本文件
```

## 🧠 架构设计

### 完整架构图

```
                    ┌──────────────────────┐    ┌───────────────────────┐    ┌──────────────────────┐    ┌──────────────────────┐
                    │      Vue 前端         │    │     iOS App (Swift)   │    │   Android App (Kotlin)│    │  Qt Desktop App (C++)
                    │   HTTP/JSON / Nginx   │    │   TCP + Protobuf      │    │   TCP + Protobuf      │    │   TCP + Protobuf      │
                    └──────────┬───────────┘    └───────────┬───────────┘    └───────────┬──────────┘    └───────────┬──────────┘
                               │                            │                            │                            │
                               └────────────────────────────┼────────────────────────────┼────────────────────────────┘
                                                           │ TCP + Protobuf
                                                           ▼
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    RPCGateway — 网关 (C++)                                     │
│                  协议转换 · 路由分发 · 限流熔断 · Token 鉴权 · **TLS termination**                │
│                   证书来源：启动时调用 CertService.DistributeCert()                               │
└────────────────────────────────────┬────────────────────────────────────────────────────────────┘
                                     │ 路由分发
                                     ▼
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                     核心框架微服务（C++）                                         │
├───────────────┬──────────────┬──────────────┬──────────────────────────┬──────────────────────┤
│ ServiceRegist.│ ConfigCenter │TracingService│   MonitorService         │   AdminConsole       │
│ :51051        │ :51052       │ :51053       │   :51054                 │   :51055             │
│ 注册发现       │ 配置管理     │ 链路追踪      │   监控告警                │   内部管理面板        │
├───────────────┴──────────────┴──────────────┴──────────────────────────┴──────────────────────┤
│                      所有核心服务启动时从 CertService 获取 mTLS 证书                          │
└────────────────────────────────────┬────────────────────────────────────────────────────────────┘
                                     │
                                     ▼
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                   业务微服务层                                                     │
├────────────────┬──────────────┬──────────────┬──────────────┬──────────────────────────────────┤
│   UserService  │   Article    │     Blog     │    Image     │           Video                   │
│    (Rust)      │    (Go)      │    (Go)      │    (C++)     │           (Go)                   │
│   :50052       │   :50053     │   :50054     │   :50055     │          :50056                  │
│  用户认证/Token │  文章CRUD/   │  博客/评论/   │  图片处理/   │       视频转码/流媒体             │
│  权限/设备管理  │  分类/标签    │  点赞/推荐    │  缩略图/转换  │       FFmpeg 调度                │
├────────────────┴──────────────┴──────────────┴──────────────┴──────────────────────────────────┤
│                      所有业务服务启动时从 CertService 获取 mTLS 证书                          │
├─────────────────────────────────────────────────────────────────────────────────────────────────┤
│               SearchService (C++ :50057)   ←→   Elasticsearch (:9200)                           │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    │ 所有服务在启动时和到期前 24h 调用：
                                    │     CertService.DistributeCert(service_name)
                                    │     → 返回 { certificate, private_key, ca_cert, expires_at }
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│                          CertService — 证书分发服务 (C++ :51058) 🔐                              │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────────────────────────────────┐  │
│  │ SSL 证书自动分发  │  │ 自动续期/吊销    │  │ Nginx 配置生成                              │  │
│  │ DistributeCert   │  │ RenewCert       │  │ 自动生成 nginx.conf + SSL 配置               │  │
│  └──────────────────┘  └──────────────────┘  └──────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘
```

### 核心框架组件（C++ 实现）

| 微服务 | 优先级 | 职责 | 并发模型 | 说明 |
|--------|--------|------|---------|------|
| **RPCGateway** | ✅ 核心 | 对外统一入口 | 异步多线程 (epoll + 线程池) | 协议转换、路由分发、限流控制、鉴权验证 |
| **ServiceRegistry** | ✅ 核心 | 服务注册发现 | 异步多线程 (epoll + 线程池) | 动态路由、负载均衡、健康检查、服务上下线 |
| **ConfigCenter** | ✅ 核心 | 配置中心 | 异步多线程 (epoll + 线程池) | 统一配置管理、热更新、配置版本控制 |
| **TracingService** | ✅ 核心 | 链路追踪 | 异步多线程 (epoll + 线程池) | 请求全链路跟踪、性能分析、故障排查 |
| **MonitorService** | ✅ 核心 | 监控告警 | 异步多线程 (epoll + 线程池) | 指标采集、告警规则、可视化面板 |
| **AdminConsole** | ✅ 核心 | 内部管理面板 | 单线程事件循环 | 管理核心框架服务（网关/注册发现/配置中心/链路追踪/监控告警）的独立 Web UI |
| **ServiceConsole** | ✅ 核心 | 业务管理面板 | 单线程事件循环 | 管理业务服务（用户/文章/博客/图片/视频/搜索）的独立 Web UI |

### 业务微服务（多语言）

| 微服务 | 语言 | 端口 | 并发模型 | 职责 |
|--------|------|------|---------|------|
| UserService | Rust | 50052 | 异步多线程 (tokio) | 用户注册/登录、Token 管理、权限控制 |
| ArticleService | Go | 50053 | goroutine 协程 | 文章 CRUD、分类管理、标签管理 |
| BlogService | Go | 50054 | goroutine 协程 | 博客管理、评论系统、点赞收藏 |
| ImageService | C++ | 50055 | 异步 I/O + CPU 线程池 | 图片像素级处理、缩略图生成、格式转换 |
| VideoService | Go | 50056 | goroutine 协程 | 视频上传、转码任务调度（FFmpeg）、流媒体分发 |
| SearchService | C++ | 50057 | 异步 I/O + 缓存层 | 全文索引、搜索排序、搜索建议 |

### 安全微服务（Rust 实现 🔐）

| 微服务 | 语言 | 端口 | 并发模型 | 职责 | 核心算法/协议 |
|--------|------|------|---------|------|-------------|
| **SecurityService** | Rust | 51057 | 异步多线程 (tokio) | 数据加密/解密、密钥管理、数字签名 | AES-256-GCM / RSA-OAEP / ECIES / Ed25519 / ECDSA / SHA-256 / SHA-3 |
| **CertService** | C++ | 51058 | 异步多线程 (epoll + 线程池) | SSL/TLS 证书自动分发、续期、Nginx 配置生成 | X.509 / ACME / Let's Encrypt |

#### 并发模型设计

| 并发模型 | 适用服务 | 说明 |
|---------|---------|------|
| **异步多线程 (epoll + 线程池)** | RPCGateway, ServiceRegistry, ConfigCenter, TracingService, MonitorService, CertService | 高并发网络 I/O 服务，使用 Boost.Asio epoll 事件驱动 + 多线程 worker 池，支撑数万并发连接 |
| **异步 I/O + CPU 线程池** | ImageService, SearchService | 混合型服务：异步接收网络请求，CPU 密集型任务（图片处理/搜索排序）交由独立线程池并行处理 |
| **单线程事件循环** | AdminConsole, ServiceConsole | 管理面板服务，连接数少（< 100）、操作频率低，单线程 Boost.Asio 事件循环即可满足需求，避免多线程竞态 |
| **goroutine 协程** | ArticleService, BlogService, VideoService (Go) | Go 语言原生 goroutine 轻量级并发，适合 I/O 密集型 CRUD 和任务调度 |
| **异步多线程 (tokio)** | UserService, SecurityService (Rust) | Rust tokio 运行时异步多线程，内存安全 + 高性能，适合安全敏感服务 |

### 端口规划说明

| 端口范围 | 服务类型 | 说明 |
|---------|---------|------|
| 50051–50057 | 业务微服务 | 网关 + 6 个业务服务（User/Article/Blog/Image/Video/Search） |
| 51051–51058 | 核心框架 + 安全 | 注册发现/配置中心/链路追踪/监控告警 + 管理面板 + SecurityService + CertService |
| 60907 | 前端 | Vue 3 开发服务器 |

---

## 📋 微服务简介

> 本章节对架构中 **16 个微服务**逐一进行详细介绍，涵盖语言选择、核心职责、技术架构和设计理念。

---

### 🏛️ 核心框架层（C++）

#### 🔷 RPCGateway — 网关服务

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 50051 |
| **优先级** | ✅ 核心 |
| **代码仓库** | `RPCGateway/` |

RPCGateway 是整个微服务架构的**流量枢纽**。职责：协议转换（HTTP↔Protobuf）、路由分发、令牌桶限流、Token 鉴权、**TLS termination**。基于 C++ epoll 事件驱动，单机支撑数万并发。TLS 证书由 CertService 独立分发，启动时调用 `DistributeCert` 获取。

---

#### 🔷 ServiceRegistry — 服务注册发现

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 51051 |
| **优先级** | ✅ 核心 |
| **代码仓库** | `ServiceRegistry/` |

ServiceRegistry 维护着整个集群的**动态服务注册表**。服务启动时注册实例并定期心跳续约，超时自动剔除。支持加权轮询/最小连接数负载均衡，`Watch` 接口实时推送上下线事件，实现无硬编码的服务动态发现与优雅上下线。

---

#### 🔷 ConfigCenter — 配置中心

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 51052 |
| **优先级** | ✅ 核心 |
| **代码仓库** | `ConfigCenter/` |

ConfigCenter 解决配置管理的三大痛点：**散落、需重启、难追溯**。所有配置项集中存储，`WatchConfig` 实时推送变更实现热更新无需重启，`GetConfigHistory` 提供版本审计轨迹支持回滚。按服务名+配置键两层命名空间隔离，避免冲突。

---

#### 🔷 TracingService — 链路追踪

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 51053 |
| **优先级** | ✅ 核心 |
| **代码仓库** | `TracingService/` |

TracingService 实现基于 **Google Dapper** 的分布式链路追踪。每个请求生成全局 TraceID 串联各服务 Span，支持按 TraceID 精确检索调用链、按条件搜索慢请求。`GetServiceMap` 自动构建服务依赖拓扑图，直观展示调用关系和流量大小，是故障排查利器。

---

#### 🔷 MonitorService — 监控告警

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 51054 |
| **优先级** | ✅ 核心 |
| **代码仓库** | `MonitorService/` |

MonitorService 是微服务架构的 **"体检中心"**。采集 CPU/内存/QPS/P99 延迟等指标，支持配置告警规则（如错误率连续 5 分钟超 5% 触发通知），多渠道推送（邮件/钉钉/企微）。`GetServiceHealth` 一键健康检查，数据对接 Grafana 实现可视化运维监控。

---

### 📊 管理面板层（C++）

#### 🟦 AdminConsole — 内部管理面板

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 51055 |
| **优先级** | ✅ 核心 |
| **代码仓库** | `AdminConsole/` |

AdminConsole 是面向**运维人员**的管控中心。五大模块：服务治理（路由/限流/实例上下线）、系统监控（CPU/内存/QPS 仪表盘）、配置管理（可视化编辑+热更新）、链路追踪（TraceID 搜索+瀑布图）、告警管理（规则配置+处理记录）。独立 Web UI，Protobuf 直连核心服务，操作全审计。

**并发模型**：单线程事件循环（Boost.Asio）。管理面板连接数少（< 100 并发）、操作频率低，单线程即可满足需求，避免多线程竞态条件和调试复杂度。

---

#### 🟦 ServiceConsole — 业务管理面板

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 51056 |
| **优先级** | ✅ 核心 |
| **代码仓库** | `ServiceConsole/` |

ServiceConsole 是面向**运营人员**的业务管控台。三大模块：用户管理（角色权限/封禁解封）、内容审核（文章/博客/图片/视频审核队列）、运营统计（用户增长/内容产出/互动数据图表）。独立 Web UI，Protobuf 直连业务微服务，操作同步审计日志。

**并发模型**：单线程事件循环（Boost.Asio）。管理面板连接数少（< 100 并发）、操作频率低，单线程即可满足需求，避免多线程竞态条件和调试复杂度。

---

### 🔐 安全层

#### 🟥 SecurityService — 加密安全服务

| 属性 | 说明 |
|------|------|
| **语言** | Rust（基于 `ring`、`rustls`、`tonic` 加密库） |
| **端口** | 51057 |
| **优先级** | ✅ 核心 |
| **代码仓库** | `SecurityService/` |

SecurityService 是整个架构的**安全基石**，选择 Rust 保障内存安全。四大模块：**数据加密**（AES-256-GCM 对称 + RSA/ECIES 非对称）、**数字签名**（Ed25519/ECDSA，90 天密钥轮换）、**密钥管理**（全生命周期自动轮换，旧密钥保留 180 天）、**审计日志**。

---

#### 🟨 CertService — 证书分发服务

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 51058 |
| **优先级** | ✅ 核心 |
| **代码仓库** | `CertService/` |

CertService 是独立的 **SSL/TLS 证书分发微服务**，Proto 接口定义于 `proto/source/cert/cert_service.proto`。作为内部 CA 为 RPCGateway 及所有微服务统一签发、续期、吊销 mTLS 证书，每个服务启动时独立调用 `DistributeCert` 获取。支持 ACME 协议对接 Let's Encrypt 自动获取公网证书，自动生成 Nginx SSL 配置文件和 `nginx.conf`。证书 7 天短生命周期，到期前 24h 自动续期，支持 CRL 吊销。

---

### 🟢 业务服务层（多语言）

#### UserService — 用户服务（Rust）

| 属性 | 说明 |
|------|------|
| **语言** | Rust |
| **端口** | 50052 |
| **类型** | 安全敏感型 |
| **代码仓库** | `UserService/` |

UserService 是平台的**用户认证中心**。Rust 保障密码哈希/Token 签发的内存安全。核心：bcrypt 密码存储、JWT Token 签发、`VerifyToken` 供网关鉴权、RBAC 权限模型、多设备管理。敏感字段（手机号/邮箱）存储前经 SecurityService 加密。

---

#### ArticleService — 文章服务（Go）

| 属性 | 说明 |
|------|------|
| **语言** | Go |
| **端口** | 50053 |
| **类型** | I/O 密集型 CRUD |
| **代码仓库** | `ArticleService/` |

ArticleService 是 **CMS 核心**（Go 实现）。Go goroutine 轻松应对文章 CRUD 的大量数据库并发读写。支持多级分类+标签组织文章，分页过滤查询，Redis 原子自增统计阅读量。服务端 Markdown 渲染防 XSS，敏感数据经 SecurityService 加密存储。

---

#### BlogService — 博客服务（Go）

| 属性 | 说明 |
|------|------|
| **语言** | Go |
| **端口** | 50054 |
| **类型** | I/O 密集型 CRUD + 社交互动 |
| **代码仓库** | `BlogService/` |

BlogService 提供**社交化博客平台**（Go 实现）。goroutine 高效处理并发互动：嵌套评论（楼中楼）、点赞（Redis Set 防重复）、个性化推荐（阅读历史+标签+作者偏好）。消息队列异步写入高并发数据，敏感操作经 UserService 鉴权。

---

#### ImageService — 图片服务（C++）

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 50055 |
| **类型** | 计算密集型 |
| **代码仓库** | `ImageService/` |

ImageService 是**高性能图片处理引擎**（C++ 实现）。C++ 直接调用 SIMD/GPU 加速像素级操作。支持客户端流式上传、多尺寸缩略图（128~1024px）、格式转换（JPEG/PNG/WebP/AVIF）。线程池并行批量处理，沙箱进程隔离防恶意图片攻击。

---

#### VideoService — 视频服务（Go）

| 属性 | 说明 |
|------|------|
| **语言** | Go |
| **端口** | 50056 |
| **类型** | 任务调度 I/O 密集型 |
| **代码仓库** | `VideoService/` |

VideoService 是**视频处理调度中心**（Go 实现）。Go goroutine+Channel 管理 FFmpeg 转码任务队列。异步流程：流式上传→FFmpeg 多格式多分辨率转码→HLS 切片分发（.m3u8+.ts）。Redis Pub/Sub 实时推送转码进度，敏感信息调用 SecurityService 加密。

---

#### SearchService — 搜索服务（C++）

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 50057 |
| **类型** | 计算密集型 + ES 集成 |
| **代码仓库** | `SearchService/` |

SearchService 是**搜索引擎上层封装**（C++ 实现）。C++ 层处理查询解析和排序算法，ES 仅做索引检索，减少网络开销。支持多字段全文检索、布尔/短语/模糊查询、TF-IDF+业务权重混合排序、前缀匹配搜索建议。按类型重建索引+增量同步。

---

### 🖥️ 前端 & 客户端

#### Vue 3 前端

| 属性 | 说明 |
|------|------|
| **技术栈** | Vue 3 + TypeScript + HTTP/JSON |
| **端口** | 60907 |
| **代码仓库** | `vue/` |

Vue 3 前端是用户端 Web 应用。Vue 3 + TypeScript 构建，通过 **HTTP/JSON** 与网关通信。protoc 编译生成 TypeScript 类型，保证前后端接口一致性。提供文章/博客/图片/视频浏览和用户登录注册等完整体验。

#### iOS App（Swift）

| 属性 | 说明 |
|------|------|
| **技术栈** | Swift + TCP/Protobuf |
| **代码仓库** | `mobile/ios/` |

iOS 原生 App 使用 Swift + SwiftUI 开发，Protobuf 序列化通过 TCP 连接网关通信。核心页面：内容信息流、文章/博客详情、图片画廊、视频播放、用户中心。响应式 UI 跨设备适配。

#### Android App（Kotlin）

| 属性 | 说明 |
|------|------|
| **技术栈** | Kotlin + TCP/Protobuf |
| **代码仓库** | `mobile/android/` |

Android 原生 App 使用 Kotlin + Jetpack Compose 开发，Protobuf 序列化通过 TCP 连接网关通信。功能覆盖与 iOS 一致，确保跨平台用户体验统一。两者均通过 SecurityService 加密敏感通信数据。

#### Qt Desktop App（C++）

| 属性 | 说明 |
|------|------|
| **技术栈** | Qt 6 + C++17 + TCP/Protobuf |
| **代码仓库** | `desktop/` |

Qt Desktop App 是高性能桌面客户端，使用 Qt 6 + C++17 开发，Protobuf 序列化通过 TCP 直连网关通信。支持 Windows/macOS/Linux 三平台，硬件加速渲染（OpenGL/Vulkan），提供内容信息流、文章/博客详情、图片画廊、视频播放、用户中心等完整功能。独立可执行文件分发，支持 AppImage/DMG/NSIS 等安装包格式。

---

## 🔌 核心框架微服务接口

### RPCGateway — 网关服务（C++，端口:50051）

对外统一入口，负责 HTTP/Protobuf 协议转换、路由分发、限流控制、鉴权验证。

| RPC | 请求 | 响应 | 说明 |
|-----|------|------|------|
| `RouteRequest` | service_name, method_name, payload, metadata | status_code, data, error_message | 路由请求到后端微服务 |
| `HttpToRpc` | method, path, headers, body, query_params | status_code, headers, body | HTTP 协议转 RPC |
| `RateLimit` | client_ip, api_path, request_count | allowed, remaining_quota, reset_time_seconds | 限流控制 |
| `Authenticate` | token, service_name, method_name | authenticated, user_id, permissions | 鉴权验证 |

### ServiceRegistry — 服务注册发现（C++，端口:51051）

动态路由、负载均衡、健康检查、服务上下线通知。

| RPC | 请求 | 响应 | 说明 |
|-----|------|------|------|
| `Register` | service_name, instance_id, host, port, metadata | success, ttl_seconds | 服务注册 |
| `Unregister` | service_name, instance_id | success, message | 服务注销 |
| `Heartbeat` | service_name, instance_id | success, ttl_seconds | 心跳续约 |
| `Discover` | service_name | instances[] (host, port, metadata, status) | 服务发现 |
| `Watch` | service_name | stream<event_type, instance> | 服务变更监听（服务端流） |
| `HealthCheck` | service_name, instance_id | status, last_heartbeat | 健康检查 |
| `ListServices` | Empty | services[] (service_name, instance_count) | 获取所有服务列表 |

### ConfigCenter — 配置中心（C++，端口:51052）

统一配置管理、热更新、配置版本控制。

| RPC | 请求 | 响应 | 说明 |
|-----|------|------|------|
| `GetConfig` | service_name, config_key | config_value, version, updated_at | 获取配置 |
| `SetConfig` | service_name, config_key, config_value | version, success | 设置配置 |
| `DeleteConfig` | service_name, config_key | success, message | 删除配置 |
| `WatchConfig` | service_name, config_key | stream<config_key, config_value, version> | 配置变更监听（服务端流） |
| `ListConfigs` | service_name | configs[] (config_key, config_value, version) | 获取服务所有配置 |
| `GetConfigHistory` | service_name, config_key | history[] (version, config_value, updated_at, updated_by) | 配置变更历史 |

### TracingService — 链路追踪（C++，端口:51053）

请求全链路跟踪、性能分析、故障排查。

| RPC | 请求 | 响应 | 说明 |
|-----|------|------|------|
| `ReportSpan` | trace_id, span_id, parent_span_id, service_name, method_name, start_time, end_time, status, tags | success | 上报 Span |
| `QueryTrace` | trace_id | spans[] (span_id, service_name, method_name, duration, status) | 查询链路 |
| `SearchTraces` | service_name, method_name, start_time, end_time, min_duration, max_duration, status | traces[] (trace_id, root_service, duration, span_count) | 搜索链路 |
| `GetServiceMap` | Empty | services[] (service_name, dependencies[]) | 获取服务依赖拓扑图 |
| `GetSlowTraces` | service_name, min_duration, limit | traces[] (trace_id, duration, method_name) | 获取慢请求链路 |

### MonitorService — 监控告警（C++，端口:51054）

指标采集、告警规则、可视化面板。

| RPC | 请求 | 响应 | 说明 |
|-----|------|------|------|
| `ReportMetric` | service_name, metric_name, value, tags, timestamp | success | 上报指标 |
| `QueryMetric` | service_name, metric_name, start_time, end_time, aggregation | data_points[] (timestamp, value) | 查询指标 |
| `SetAlertRule` | rule_name, metric_name, condition, threshold, duration, notify_channels | rule_id, success | 设置告警规则 |
| `ListAlertRules` | service_name | rules[] (rule_id, rule_name, metric_name, condition, threshold) | 获取告警规则列表 |
| `GetAlerts` | service_name, start_time, end_time, status | alerts[] (alert_id, rule_name, metric_value, triggered_at, status) | 获取告警历史 |
| `GetServiceHealth` | service_name | status, metrics[] (metric_name, value, status) | 获取服务健康状态 |

---

## 🔐 SecurityService — 加密安全服务（Rust，端口:51057）

全链路安全微服务，提供敏感数据端到端加密、密钥生命周期管理、数字签名验签。

> **为什么用 Rust？** 安全服务是所有微服务中最不能出内存安全问题的环节。Rust 的所有权系统和零成本抽象保证了加密算法的高性能执行，同时彻底杜绝了缓冲区溢出、空指针解引用等内存安全漏洞。

### 数据加密 — 保护业务敏感数据

| RPC | 请求 | 响应 | 说明 | 算法 |
|-----|------|------|------|------|
| `Encrypt` | plaintext, encryption_key_id, aad | ciphertext, iv, tag, key_id | 对称加密 | AES-256-GCM（带附加认证数据 AAD） |
| `Decrypt` | ciphertext, iv, tag, encryption_key_id, aad | plaintext | 对称解密 | AES-256-GCM |
| `AsymmetricEncrypt` | plaintext, public_key_id | ciphertext, key_id | 非对称加密 | RSA-OAEP（兼容）/ ECIES（性能优先） |
| `AsymmetricDecrypt` | ciphertext, private_key_id | plaintext, key_id | 非对称解密 | RSA-OAEP / ECIES |
| `HashData` | data, algorithm | hash, algorithm | 数据哈希 | SHA-256 / SHA-3（可配置） |
| `Hmac` | data, secret_key_id, algorithm | hmac, algorithm | 消息认证码 | HMAC-SHA256 / HMAC-SHA3 |

### 数字签名 — 保障数据完整性与不可否认性

| RPC | 请求 | 响应 | 说明 | 算法 |
|-----|------|------|------|------|
| `Sign` | data, signing_key_id, algorithm | signature, key_id, algorithm | 数字签名 | Ed25519（性能优先）/ ECDSA P-256（兼容优先） |
| `Verify` | data, signature, public_key_id, algorithm | valid, key_id | 签名验证 | Ed25519 / ECDSA P-256 |

### 密钥管理 — 密钥全生命周期安全管控

| RPC | 请求 | 响应 | 说明 |
|-----|------|------|------|
| `GenerateKey` | key_type, key_size, metadata | key_id, public_key, created_at | 生成密钥对（支持对称/非对称/签名密钥） |
| `RotateKey` | key_id | new_key_id, rotated_at | 密钥轮换（旧密钥标记为只解密，新密钥用于加密） |

**密钥轮换策略**：
- 数据加密密钥：每 30 天自动轮换
- 签名密钥：每 90 天自动轮换
- 密钥版本化：旧密钥保留 180 天用于解密历史数据

### 加密层级设计（纵深防御模型）

```
数据分层加密模型：
┌─────────────────────────────────────────────────────────────────────┐
│  第一层：业务层 — SecurityService 字段级加密                        │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │  适用场景：用户手机号、身份证、银行卡、医疗记录等 PII 数据      │   │
│  │  加密方式：AES-256-GCM，每次加密生成随机 IV                   │   │
│  │  密钥策略：每 30 天自动轮换，旧密钥保留 180 天用于解密          │   │
│   │  调用方式：业务服务通过 RPC 调用 SecurityService.Encrypt()    │   │
│  └─────────────────────────────────────────────────────────────┘   │
├─────────────────────────────────────────────────────────────────────┤
│  第二层：传输层 — 服务间 mTLS 双向认证                             │
│  ┌─────────────────────────────────────────────────────────────┐   │
│   │  适用场景：所有微服务间的 RPC 通信                           │   │
│  │  认证方式：双向 TLS，客户端和服务端都需要出示证书               │   │
│  │  证书周期：7 天自动轮换，CA 统一签发                          │   │
│  │  加密套件：TLS 1.3 + X25519 + AES-256-GCM                   │   │
│  └─────────────────────────────────────────────────────────────┘   │
├─────────────────────────────────────────────────────────────────────┤
│  第三层：存储层 — 数据静态加密                                     │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │  数据库层：MySQL TDE（透明数据加密），表空间级自动加密          │   │
│  │  文件存储：AES-256-CBC 加密，适用于图片/视频等大文件           │   │
│  │  密钥管理：主密钥由 HSM 保护，数据密钥由 SecurityService 管理  │   │
│  └─────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────┘

安全调用流程示例（用户查询敏感数据）：
┌──────────┐    ┌─────────────┐    ┌──────────────────┐    ┌──────────┐
│  前端     │───▶│  RPCGateway │───▶│  业务服务(Go)     │───▶│  MySQL   │
│          │    │  (C++)      │    │  (ArticleService) │    │          │
│  用户查询  │    │  mTLS验证    │    │  查询数据          │    │  TDE解密  │
│  手机号   │    │  Token鉴权   │    │  ┌─────────────┐  │    │          │
│          │    │              │    │  │ SecurityClt │  │    │          │
│          │    │              │    │  └──────┬──────┘  │    │          │
└──────────┘    └─────────────┘    └─────────┼─────────┘    └──────────┘
                                             │
                                    ┌────────▼────────┐
                                    │ SecurityService  │
                                    │  (Rust) 🔐       │
                                    │  ┌────────────┐  │
                                    │  │ AES-256-GCM │  │
                                    │  │ 字段级解密   │  │
                                    │  └────────────┘  │
                                    └─────────────────┘
```

---

## 🔌 业务微服务接口

### UserService — 用户服务（Rust，端口:50052）

| RPC | 请求 | 响应 | 说明 |
|-----|------|------|------|
| `Register` | username, password, email | user_id, token, created_at | 用户注册 |
| `Login` | username, password | token, user_info, expires_at | 用户登录 |
| `Logout` | token, device_id | success, message | 注销登录 |
| `GetUserInfo` | user_id | user_id, username, avatar, email, permission, created_at | 获取用户信息 |
| `UpdateUserInfo` | user_id, username, avatar, email | success, message | 更新用户信息 |
| `ListDevices` | user_id | devices[] | 获取登录设备列表 |
| `RemoveDevice` | user_id, device_id | success, message | 移除登录设备 |
| `VerifyToken` | token | valid, user_id, permissions | Token 验证 |

### ArticleService — 文章服务（Go，端口:50053）

| RPC | 请求 | 响应 | 说明 |
|-----|------|------|------|
| `CreateArticle` | title, content, category_id, tags[], author_id | article_id, created_at | 创建文章 |
| `GetArticle` | article_id | article_id, title, content, category, tags[], author, created_at, updated_at, view_count | 获取文章详情 |
| `UpdateArticle` | article_id, title, content, category_id, tags[] | success, updated_at | 更新文章 |
| `DeleteArticle` | article_id | success, message | 删除文章 |
| `ListArticles` | page, page_size, category_id, tag_id | articles[], total, page, page_size | 文章列表（分页） |
| `GetCategories` | Empty | categories[] | 获取分类列表 |
| `CreateCategory` | name, description | category_id, created_at | 创建分类 |
| `GetTags` | Empty | tags[] | 获取标签列表 |

### BlogService — 博客服务（Go，端口:50054）

| RPC | 请求 | 响应 | 说明 |
|-----|------|------|------|
| `CreatePost` | title, content, author_id, tags[] | post_id, created_at | 创建博客文章 |
| `GetPost` | post_id | post_id, title, content, author, tags[], created_at, updated_at, like_count, comment_count | 获取博客详情 |
| `UpdatePost` | post_id, title, content, tags[] | success, updated_at | 更新博客 |
| `DeletePost` | post_id | success, message | 删除博客 |
| `ListPosts` | page, page_size, tag_id, author_id | posts[], total | 博客列表（分页） |
| `AddComment` | post_id, author_id, content | comment_id, created_at | 添加评论 |
| `ListComments` | post_id, page, page_size | comments[], total | 评论列表 |
| `DeleteComment` | comment_id, author_id | success, message | 删除评论 |
| `LikePost` | post_id, user_id | success, like_count | 点赞博客 |
| `UnlikePost` | post_id, user_id | success, like_count | 取消点赞 |
| `GetRecommendations` | user_id, page, page_size | posts[], total | 博客推荐 |

### ImageService — 图片服务（C++，端口:50055）

| RPC | 请求 | 响应 | 说明 |
|-----|------|------|------|
| `UploadImage` | stream<file_name, content, chunk_index, total_chunks> | image_id, url, thumbnail_url, size_bytes, format | 图片上传（客户端流） |
| `GetImage` | image_id | image_id, url, thumbnail_url, format, width, height, size_bytes, created_at | 获取图片信息 |
| `DeleteImage` | image_id | success, message | 删除图片 |
| `ListImages` | user_id, page, page_size | images[], total | 图片列表 |
| `ProcessImage` | image_id, operations[] | image_id, url, thumbnail_url | 图片处理 |
| `GenerateThumbnail` | image_id, width, height | thumbnail_url | 生成缩略图 |

### VideoService — 视频服务（Go，端口:50056）

| RPC | 请求 | 响应 | 说明 |
|-----|------|------|------|
| `UploadVideo` | stream<file_name, content, chunk_index, total_chunks> | video_id, status | 视频上传（客户端流） |
| `GetVideoInfo` | video_id | video_id, title, url, duration, format, resolution, size_bytes, status, created_at | 获取视频信息 |
| `DeleteVideo` | video_id | success, message | 删除视频 |
| `ListVideos` | user_id, page, page_size | videos[], total | 视频列表 |
| `StartTranscoding` | video_id, target_formats[], resolutions[] | job_id, status | 启动转码任务 |
| `GetTranscodingStatus` | job_id | job_id, status, progress, output_formats[] | 查询转码进度 |
| `GetStreamUrl` | video_id, format, resolution | stream_url, expires_at | 获取流媒体播放地址 |
| `GetVideoChunk` | video_id, chunk_index, format, resolution | stream<content, chunk_index, total_chunks> | 获取视频切片（服务端流） |

### SearchService — 搜索服务（C++，端口:50057）

| RPC | 请求 | 响应 | 说明 |
|-----|------|------|------|
| `Search` | query, type, page, page_size, filters | results[], total, elapsed_ms | 全文搜索 |
| `IndexDocument` | document_id, type, title, content, tags[], metadata | success, indexed_at | 索引文档 |
| `RemoveIndex` | document_id, type | success, message | 移除索引 |
| `RebuildIndex` | type | job_id, status | 重建索引 |
| `GetSearchSuggestions` | prefix, type, limit | suggestions[] | 搜索建议 |
| `GetTrendingSearches` | limit | trending[] | 热门搜索 |

---

## 🚀 快速开始

### 前置依赖

| 组件 | 版本要求 | 说明 |
|------|----------|------|
| C++ 编译器 | C++17（GCC 8+ / MSVC 2019+） | 核心框架编译 |
| CMake | 3.10+ | C++ 构建系统 |
| Boost | 1.83+（已包含在仓库中） | C++ 微服务依赖（Beast/Asio/JSON/Algorithm），header-only 无需单独编译 |
| MySQL | 8.0+ | 数据库 |
| Protobuf | 3.15+ | 序列化协议（protoc 编译器） |
| Rust | 1.70+ | UserService（安全敏感认证服务）、SecurityService（加密安全服务） |
| Go | 1.21+ | ArticleService、BlogService、VideoService（I/O 密集型服务） |
| Node.js | 18+ | 前端构建 |

### 1️⃣ 克隆仓库

```bash
git clone --recursive https://github.com/jyoushitou/WebServer.git
cd WebServer
git submodule update --init --recursive
```

> **关于 Boost 库**：`boost/` 目录已直接包含在仓库中（header-only 模式），所有 C++ 微服务（RPCGateway、ServiceRegistry、ConfigCenter、TracingService、MonitorService、AdminConsole、ServiceConsole、ImageService、SearchService、CertService）均依赖此 Boost 库。
>
> 核心依赖组件：
> - **Boost.Beast** — HTTP/WebSocket 协议实现，用于 RPCGateway 的 HTTP 协议转换
> - **Boost.Asio** — 异步网络 I/O 框架，所有 C++ 微服务的网络通信基础
> - **Boost.JSON** — JSON 解析与序列化，用于配置解析和日志格式化
> - **Boost.Algorithm** — 算法扩展（字符串处理、集合操作等）
>
> 由于采用 header-only 方式，无需单独编译 Boost 库，CMake 配置中通过 `add_subdirectory(boost)` 或 `target_include_directories` 直接引用即可。

### 2️⃣ 检查前置工具

```bash
# 检查 protoc 是否安装
protoc --version    # 需要 3.15+

# 检查 C++ 编译器
g++ --version       # 需要 C++17

# 检查 Rust 工具链（UserService / SecurityService 需要）
rustc --version     # 需要 1.70+

# 检查 Go 工具链（ArticleService / BlogService / VideoService 需要）
go version          # 需要 1.21+

# 检查 Node.js（Vue 前端构建需要）
node --version      # 需要 18+
```

### 2️⃣ 编译 Proto 库（生成多语言代码）

#### 什么是 Proto 驱动开发？

本项目所有微服务之间的通信接口，**全部由 `.proto` 文件统一定义**，然后通过 `protoc` 编译器生成多语言的 Protobuf 序列化代码。服务间通信使用自定义 TCP 帧协议（4 字节长度前缀 + Protobuf 二进制数据）。

这种方式的好处是：

1. **接口即契约** — 一个 `.proto` 文件同时约束多语言，保证接口完全一致
2. **类型安全** — 编译时检查类型，避免运行时序列化错误
3. **零运行时开销** — Protobuf 二进制序列化，比 JSON 快 10-100 倍
4. **多语言原生支持** — 每种语言生成对应风格的代码（C++ 类、Go struct、Rust struct 等）

#### 目录结构说明

```
proto/
├── source/                          # Proto 源文件（接口定义）
│   ├── common/                      # 公共类型、枚举、错误码
│   │   └── common.proto            # RequestId、Pagination、ErrorCode
│   ├── gateway/                     # 网关服务接口
│   │   └── gateway.proto           # RouteRequest、HttpToRpc、RateLimit
│   ├── registry/                    # 服务注册发现接口
│   │   └── registry.proto          # Register、Discover、Watch
│   ├── config/                      # 配置中心接口
│   │   └── config.proto            # GetConfig、WatchConfig
│   ├── security/                    # 🔐 加密安全服务接口（新增）
│   │   └── security.proto          # Encrypt、Sign、GenerateKey、Certificate 等
│   ├── user/                        # 用户服务接口
│   ├── article/                     # 文章服务接口
│   ├── blog/                        # 博客服务接口
│   ├── image/                       # 图片服务接口
│   ├── video/                       # 视频服务接口
│   ├── search/                      # 搜索服务接口
│   ├── tracing/                     # 链路追踪接口
│   ├── monitor/                     # 监控告警接口
│   ├── frontend/                    # 前端服务接口
│   ├── admin_console/               # 内部管理面板接口
│   └── service_console/             # 业务管理面板接口
└── build/                           # 编译输出
    └── generated/
        ├── cpp/                     # C++ 生成代码
        ├── go/                      # Go 生成代码
        └── rust/                    # Rust 生成代码
```

#### 生成 C++ 代码（核心框架）

```bash
cd proto/source && mkdir -p ../build && cd ../build
cmake ../source && cmake --build .
echo "✅ C++ Protobuf 代码生成完成"
# 生成位置：proto/build/generated/cpp/
```

#### 生成 Go 代码（业务服务）

```bash
# 先安装 protoc 的 Go 插件
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest

# 编译 Go 的 Protobuf 序列化代码
protoc --go_out=../build/generated/go \
  -I ../source $(find ../source -name "*.proto")
echo "✅ Go Protobuf 代码生成完成"
# 生成位置：proto/build/generated/go/
```

#### 生成 Rust 代码（安全服务）

```bash
# 先安装 protoc 的 Rust 插件
cargo install protoc-gen-prost

# 编译 Rust 的 Protobuf 序列化代码
protoc --prost_out=../build/generated/rust \
  -I ../source $(find ../source -name "*.proto")
echo "✅ Rust Protobuf 代码生成完成"
# 生成位置：proto/build/generated/rust/
```

#### 一键生成所有语言代码

```bash
cd proto

# 配置：源码目录
PROTO_DIR=source
OUT_DIR=build/generated
mkdir -p $OUT_DIR/cpp $OUT_DIR/go $OUT_DIR/rust

# 查找所有 .proto 文件
PROTO_FILES=$(find $PROTO_DIR -name "*.proto")

echo "🚀 开始编译所有 .proto 文件..."

# ─── C++ ───────────────────────────────
protoc --cpp_out=$OUT_DIR/cpp \
  -I $PROTO_DIR $PROTO_FILES
echo "  ✅ C++    → $OUT_DIR/cpp/"

# ─── Go ─────────────────────────────────
protoc --go_out=$OUT_DIR/go \
  -I $PROTO_DIR $PROTO_FILES
echo "  ✅ Go     → $OUT_DIR/go/"

# ─── Rust ───────────────────────────────
protoc --prost_out=$OUT_DIR/rust \
  -I $PROTO_DIR $PROTO_FILES
echo "  ✅ Rust   → $OUT_DIR/rust/"

echo "🎉 所有语言代码生成完成！"
```

> **注意**：
> - protoc 只生成序列化代码（`.pb.h` / `.pb.go` / `.rs`），**不生成 RPC 通信骨架**
> - 服务间通信的 RPC 框架由各服务自行实现 TCP 帧协议 + 方法路由
> - 生成代码通常不提交到 Git 仓库，而是在 CI/CD 构建流程中自动生成
> - Go 和 Rust 的 protoc 插件首次使用前需要手动安装（见上方命令）

#### security.proto 示例（SecurityService 接口定义片段）

```protobuf proto/source/security/security.proto
syntax = "proto3";
package security;
option go_package = "proto/build/generated/go/security";

// 加密安全服务 — 全链路加密、密钥管理、数字签名
service SecurityService {
  // ── 数据加密 ──
  rpc Encrypt(EncryptRequest) returns (EncryptResponse);
  rpc Decrypt(DecryptRequest) returns (DecryptResponse);
  rpc AsymmetricEncrypt(AsymmetricEncryptRequest) returns (AsymmetricEncryptResponse);
  rpc AsymmetricDecrypt(AsymmetricDecryptRequest) returns (AsymmetricDecryptResponse);

  // ── 数字签名 ──
  rpc Sign(SignRequest) returns (SignResponse);
  rpc Verify(VerifyRequest) returns (VerifyResponse);

  // ── 密钥管理 ──
  rpc GenerateKey(GenerateKeyRequest) returns (GenerateKeyResponse);
  rpc RotateKey(RotateKeyRequest) returns (RotateKeyResponse);

  // ── 哈希与消息认证 ──
  rpc HashData(HashDataRequest) returns (HashDataResponse);
  rpc Hmac(HmacRequest) returns (HmacResponse);
}

message EncryptRequest {
  bytes plaintext = 1;
  string encryption_key_id = 2;
  bytes aad = 3;  // 附加认证数据（Additional Authenticated Data）
}

message EncryptResponse {
  bytes ciphertext = 1;
  bytes iv = 2;          // 初始化向量
  bytes tag = 3;         // GCM 认证标签
  string key_id = 4;     // 使用的密钥 ID
  string key_version = 5;
}
```

### 3️⃣ 启动核心框架（C++）

```bash
# 启动服务注册发现
cd ServiceRegistry && mkdir build && cd build
cmake .. && cmake --build . && ./ServiceRegistry

# 启动配置中心
cd ConfigCenter && mkdir build && cd build
cmake .. && cmake --build . && ./ConfigCenter

# 启动网关
cd RPCGateway && mkdir build && cd build
cmake .. && cmake --build . && ./RPCGateway

# 启动链路追踪
cd TracingService && mkdir build && cd build
cmake .. && cmake --build . && ./TracingService

# 启动监控告警
cd MonitorService && mkdir build && cd build
cmake .. && cmake --build . && ./MonitorService

# 启动内部管理面板
cd AdminConsole && mkdir build && cd build
cmake .. && cmake --build . && ./AdminConsole

# 启动业务管理面板
cd ServiceConsole && mkdir build && cd build
cmake .. && cmake --build . && ./ServiceConsole
```

### 4️⃣ 启动业务微服务

```bash
# Rust 服务（安全敏感）
cd UserService && cargo build --release && ./target/release/user-service
cd SecurityService && cargo build --release && ./target/release/security-service

# C++ 服务（高性能计算密集型）
cd ImageService && mkdir build && cd build
cmake .. && cmake --build . && ./ImageService
cd SearchService && mkdir build && cd build
cmake .. && cmake --build . && ./SearchService

# Go 服务（I/O 密集型）
cd ArticleService && go build -o article-service . && ./article-service
cd BlogService && go build -o blog-service . && ./blog-service
cd VideoService && go build -o video-service . && ./video-service
```

### 5️⃣ 启动前端

```bash
cd vue && npm install && npm run dev
```

---

## ⚙️ 配置说明

### 服务器端口

#### 端口分配原则
| 端口范围 | 服务类型 | 说明 |
|---------|---------|------|
| 50051–50057 | 业务微服务端口 | 网关 + 6 个业务服务 |
| 51051–51057 | 核心框架 + 安全端口 | 注册发现/配置中心/链路追踪/监控告警 + 管理面板 + SecurityService |
| 60907 | 前端端口 | Vue 3 开发服务器 |
| 9200 | 搜索引擎 | Elasticsearch |

#### 端口明细

| 服务 | 语言 | 端口 | 类型 | 所属层级 |
|------|------|------|------|---------|
| RPCGateway | C++ | 50051 | 核心框架 | 网关层 |
| ServiceRegistry | C++ | 51051 | 核心框架 | 基础设施层 |
| ConfigCenter | C++ | 51052 | 核心框架 | 基础设施层 |
| TracingService | C++ | 51053 | 核心框架 | 可观测性层 |
| MonitorService | C++ | 51054 | 核心框架 | 可观测性层 |
| AdminConsole | C++ | 51055 | 核心框架 | 管理面板层 |
| ServiceConsole | C++ | 51056 | 核心框架 | 管理面板层 |
| SecurityService | Rust | 51057 | 安全服务 🔐 | 安全层 |
| CertService | C++ | 51058 | 证书服务 🔐 | 安全层 |
| UserService | Rust | 50052 | 业务服务 | 业务层 |
| ArticleService | Go | 50053 | 业务服务 | 业务层 |
| BlogService | Go | 50054 | 业务服务 | 业务层 |
| ImageService | C++ | 50055 | 业务服务 | 业务层 |
| VideoService | Go | 50056 | 业务服务 | 业务层 |
| SearchService | C++ | 50057 | 业务服务 | 业务层 |
| Vue 前端 | JS | 60907 | 前端 | 前端层 |
| Elasticsearch | Java | 9200 | 搜索引擎 | 数据层 |

---

---

## 📋 开发计划

### 阶段一：单体架构 ✅ 已归档
- [x] C++ 原生 Socket HTTP 服务器
- [x] HTTP 协议手动解析、多线程并发处理
- [x] MySQL 数据库直连、Vue 3 前端界面
- [x] 用户认证系统（Token）、异步任务处理
- [x] 归档仓库：https://github.com/jyoushitou/WebServer_cpp.git

### 阶段二：微服务架构 🔄 进行中

#### Proto 仓库搭建 ✅ 已完成
- [x] Proto 独立仓库搭建（proto/ 子仓库）
- [x] Proto 目录结构规划（common/gateway/registry/config/tracing/monitor/user/article/blog/image/video/search/frontend/admin_console/service_console）
- [x] 公共类型定义（RequestId、Pagination、ErrorCode 等通用 message）
- [x] **security/** 目录规划 — 加密安全服务接口（新增）

#### Proto 多语言编译 ✅ 已完成
- [x] CMake 构建脚本 — 编译 C++ Protobuf 代码
- [x] protoc 编译命令 — 支持 C++ / Go / Rust 三种语言
- [x] 各语言 protoc 编译说明（--cpp_out / --go_out / --rust_out）

#### AdminConsole 内部管理面板 (C++)
- [ ] **Admin Web UI 前端** — 独立 HTML + JS 管理面板
  - [ ] 服务治理页面 — 查看所有核心服务状态、启停控制
  - [ ] 系统监控仪表盘 — CPU/内存/QPS/延迟实时展示
  - [ ] 配置管理页面 — 查看/编辑/推送配置
  - [ ] 健康检查页面 — 各服务健康状态概览
- [ ] **后端对接**
  - [ ] 对接 RPCGateway 网关管理 — 查看路由规则、限流配置
  - [ ] 对接 ServiceRegistry 注册发现管理 — 查看实例列表、手动上下线
  - [ ] 对接 ConfigCenter 配置中心管理 — 配置 CRUD、版本回溯
  - [ ] 对接 TracingService 链路追踪管理 — 搜索追踪、查看 Span 详情
  - [ ] 对接 MonitorService 监控告警管理 — 设置告警规则、查看告警历史
- [ ] **安全接入**
  - [ ] Admin 登录认证对接 UserService
  - [ ] 操作审计日志

#### ServiceConsole 业务管理面板 (C++)
- [ ] **Service Web UI 前端** — 独立 HTML + JS 管理面板
  - [ ] 用户管理页面 — 用户列表、权限管理、封禁/解封
  - [ ] 内容审核页面 — 文章/博客/图片/视频内容审核队列
  - [ ] 运营统计页面 — 用户增长、内容产出、互动数据
- [ ] **后端对接**
  - [ ] 对接 UserService 用户管理 — 用户 CRUD、权限分配
  - [ ] 对接 ArticleService 文章管理 — 文章审核、分类管理
  - [ ] 对接 BlogService 博客管理 — 博客审核、评论管理
  - [ ] 对接 ImageService 图片管理 — 图片列表、删除违规图片
  - [ ] 对接 VideoService 视频管理 — 视频审核、转码任务查看
  - [ ] 对接 SearchService 搜索管理 — 索引状态、重建索引触发
- [ ] **安全接入**
  - [ ] Service 登录认证对接 UserService
  - [ ] 操作审计日志

#### SecurityService 加密安全服务 (Rust 🔐)
- [ ] **Proto 接口定义** — 定义 security.proto
  - [ ] Encrypt/Decrypt — AES-256-GCM 对称加密接口
  - [ ] AsymmetricEncrypt/AsymmetricDecrypt — RSA-OAEP/ECIES 非对称加密接口
  - [ ] Sign/Verify — Ed25519/ECDSA 数字签名接口
  - [ ] GenerateKey/RotateKey — 密钥管理接口
  - [ ] HashData/Hmac — 哈希与消息认证码接口
- [ ] **加密实现**
  - [ ] AES-256-GCM 对称加密模块（使用 Rust `ring` 库）
  - [ ] RSA-OAEP / ECIES 非对称加密模块
  - [ ] Ed25519 / ECDSA P-256 数字签名模块
  - [ ] SHA-256 / SHA-3 哈希模块
  - [ ] HMAC 消息认证码模块
- [ ] **密钥管理**
  - [ ] 密钥生成与存储（内存加密 + 持久化到数据库）
  - [ ] 密钥版本化管理
  - [ ] 自动轮换调度器（30 天加密密钥 / 90 天签名密钥）
  - [ ] 密钥吊销机制
- [ ] **mTLS 证书管理**
  - [ ] 内部 CA 根证书生成
  - [ ] 服务证书签发（7 天有效期）
  - [ ] 自动续期调度器（到期前 24 小时自动续期）
  - [ ] 证书吊销列表（CRL）管理
- [ ] **集成对接**
  - [ ] 对接 RPCGateway 网关 — 敏感字段自动加密/解密
  - [ ] 对接 AdminConsole — 密钥管理面板、证书状态查看
  - [ ] 对接 ServiceRegistry — 安全服务注册发现
  - [ ] 所有业务服务集成 SecurityService 客户端 SDK

#### CertService 证书分发服务 (C++ 🔐)
- [ ] **Proto 接口定义** — 定义 cert_service.proto
  - [ ] DistributeCert — SSL 证书分发接口
- [ ] **证书管理实现**
  - [ ] 内部 CA 根证书生成
  - [ ] 服务证书签发（7 天有效期）
  - [ ] 自动续期调度器（到期前 24 小时自动续期）
  - [ ] 证书吊销列表（CRL）管理
  - [ ] ACME 协议对接 Let's Encrypt
- [ ] **Nginx 集成**
  - [ ] 自动生成 nginx.conf 文件
  - [ ] SSL 配置模板管理
  - [ ] 热重载 Nginx 配置
- [ ] **集成对接**
  - [ ] 对接 RPCGateway — TLS 证书分发
  - [ ] 对接 AdminConsole — 证书状态查看、手动续期
  - [ ] 对接 ServiceRegistry — 证书服务注册发现

#### 业务微服务
- [ ] **UserService**（Rust — 安全敏感，内存安全优势）
  - [ ] 用户注册/登录、Token 签发
  - [ ] Token 验证（对接 RPCGateway 鉴权中间件）
  - [ ] 权限角色管理（RBAC）
  - [ ] 多设备登录管理
- [ ] **ArticleService**（Go — I/O 密集型 CRUD）
  - [ ] 文章 CRUD
  - [ ] 分类/标签管理
  - [ ] 分页查询
- [ ] **BlogService**（Go — I/O 密集型 CRUD）
  - [ ] 博客 CRUD
  - [ ] 评论系统
  - [ ] 点赞/收藏
  - [ ] 博客推荐算法
- [ ] **ImageService**（C++ — 像素级高性能处理）
  - [ ] 图片上传（客户端流式）
  - [ ] 缩略图生成（多尺寸）
  - [ ] 格式转换（JPEG/PNG/WebP/AVIF）
- [ ] **VideoService**（Go — 转码任务调度、流媒体 I/O）
  - [ ] 视频上传（客户端流式）
  - [ ] FFmpeg 转码任务调度
  - [ ] HLS 切片分发
  - [ ] 转码进度查询
- [ ] **SearchService**（C++ — ES 集成、高性能排序）
  - [ ] Elasticsearch 集成
  - [ ] 全文搜索与排序
  - [ ] 搜索建议（前缀匹配）
  - [ ] 索引管理（重建/同步）

#### 前端 & 移动端

##### 阶段一：Vue 3 Web 前端（进行中）
- [ ] **Vue 前端** HTTP/JSON 接入
  - [ ] 文章浏览
  - [ ] 搜索页面
  - [ ] 博客
  - [ ] 评论互动页面
  - [ ] 图片
  - [ ] 视频展示页面
  - [ ] 用户登录
  - [ ] 注册页面
  - [ ] **技术栈**：Vue 3 + TypeScript + Pinia 状态管理 + Vue Router
  - [ ] **性能优化**
    - [ ] 组件懒加载（Vue Router 动态导入）
    - [ ] 图片懒加载（Intersection Observer）
    - [ ] 虚拟滚动（长列表优化）
    - [ ] 服务端渲染（SSR）支持（Nuxt 3 迁移规划）
  - [ ] **UI/UX**
    - [ ] 响应式布局（移动端/平板/桌面自适应）
    - [ ] 深色模式支持
    - [ ] 动画过渡（Vue Transition + GSAP）
    - [ ] 骨架屏加载状态
  - [ ] **安全集成**
    - [ ] Token 自动刷新（Axios 拦截器）
    - [ ] XSS 防护（DOMPurify 富文本过滤）
    - [ ] CSRF Token 保护
  - [ ] **打包分发**
    - [ ] Docker 容器化部署（Nginx + 多阶段构建）
    - [ ] CDN 静态资源分发
    - [ ] PWA 离线支持（Service Worker）
    - [ ] 自动化构建（GitHub Actions + Vercel/Netlify）

##### 阶段二：移动端原生 App（规划中）
- [ ] **iOS App**（Swift）
  - [ ] TCP/Protobuf 客户端集成
  - [ ] 文章/博客/图片/视频浏览
  - [ ] 用户登录/注册
  - [ ] **技术栈**：Swift + SwiftUI + Combine
  - [ ] **核心特性**
    - [ ] 原生性能（Metal 渲染加速）
    - [ ] 离线缓存（Core Data + 磁盘缓存）
    - [ ] 推送通知（APNs）
    - [ ] Widget 小组件（iOS 14+）
    - [ ] 深色模式 + 动态字体
  - [ ] **安全集成**
    - [ ] mTLS 双向认证连接网关
    - [ ] 本地敏感数据加密（Keychain + AES-256-GCM）
    - [ ] 生物识别解锁（Face ID / Touch ID）
    - [ ] App Attest 安全验证
  - [ ] **打包分发**
    - [ ] App Store Connect 发布
    - [ ] TestFlight 内测分发
    - [ ] 代码混淆（Swift Obfuscator）
    - [ ] 签名证书管理（Apple Developer Program）

- [ ] **Android App**（Kotlin）
  - [ ] **技术选型**：Kotlin + Jetpack Compose + TCP/Protobuf 直连网关
  - [ ] **核心页面**
    - [ ] 内容信息流 — 文章/博客/图片/视频聚合浏览
    - [ ] 文章/博客详情页 — 富文本渲染、评论互动
    - [ ] 图片画廊 — 基于 Coil 的高性能图片加载、缩放、分享
    - [ ] 视频播放器 — 基于 ExoPlayer 的流媒体播放、倍速、画中画
    - [ ] 用户中心 — 个人资料、收藏、历史记录、设置
  - [ ] **高性能特性**
    - [ ] 异步网络 I/O（OkHttp + Protobuf 序列化）
    - [ ] 本地缓存（Room 离线数据 + Coil 图片磁盘缓存）
    - [ ] 协程并发请求（Kotlin Coroutines + Flow 响应式数据流）
    - [ ] 分页加载（Paging 3 库实现无限滚动）
  - [ ] **安全集成**
    - [ ] mTLS 双向认证连接网关
    - [ ] 本地敏感数据加密存储（EncryptedSharedPreferences + AES-256-GCM）
    - [ ] 生物识别解锁（Biometric Prompt 指纹/面部识别）
    - [ ] 安全启动验证（App Integrity API 校验）
  - [ ] **UI/UX 设计**
    - [ ] Material 3 设计语言（Material You 动态主题）
    - [ ] 深色模式支持
    - [ ] 响应式布局（手机/平板自适应）
    - [ ] 动画过渡（Jetpack Compose 动画系统）
  - [ ] **跨版本兼容**
    - [ ] 最低支持 Android 8.0 (API 26)
    - [ ] 目标 SDK Android 14 (API 34)
    - [ ] 兼容平板、折叠屏设备
  - [ ] **打包分发**
    - [ ] AAB (Android App Bundle) 发布 Google Play
    - [ ] APK 侧载分发
    - [ ] ProGuard/R8 代码混淆
    - [ ] 签名证书管理（Google Play App Signing）

##### 阶段三：Qt Desktop App 📋 规划中
- [ ] **Qt Desktop App**（C++ — 高性能桌面客户端）
  - [ ] **技术选型**：Qt 6 + C++17 + TCP/Protobuf 直连网关
  - [ ] **核心页面**
    - [ ] 内容信息流 — 文章/博客/图片/视频聚合浏览
    - [ ] 文章/博客详情页 — 富文本渲染、评论互动
    - [ ] 图片画廊 — 高性能图片加载、缩放、旋转
    - [ ] 视频播放器 — 基于 Qt Multimedia 的本地/流媒体播放
    - [ ] 用户中心 — 个人资料、收藏、历史记录
  - [ ] **高性能特性**
    - [ ] 硬件加速渲染（OpenGL/Vulkan 后端）
    - [ ] 异步网络 I/O（Qt Network + Protobuf 序列化）
    - [ ] 本地缓存（SQLite 离线数据 + 图片/视频磁盘缓存）
    - [ ] 多线程并行加载（QThreadPool 并发请求）
  - [ ] **安全集成**
    - [ ] mTLS 双向认证连接网关
    - [ ] 本地敏感数据加密存储（AES-256-GCM）
    - [ ] 安全启动验证（数字签名校验可执行文件完整性）
  - [ ] **跨平台支持**
    - [ ] Windows（MSVC 2022 + Qt 6）
    - [ ] macOS（Clang + Qt 6）
    - [ ] Linux（GCC + Qt 6）
  - [ ] **打包分发**
      - [ ] Windows — NSIS / WiX 安装包
      - [ ] macOS — DMG 磁盘映像 + 公证
      - [ ] Linux — AppImage / Snap / Flatpak
    - [ ] **独立可执行文件分发（Standalone Executable）**
      - [ ] **Windows x86_64**
        - [ ] 静态链接 Qt 6 运行时库，生成单个 `.exe` 文件
        - [ ] 使用 UPX 压缩减小体积
        - [ ] 数字签名（Authenticode 签名证书）
        - [ ] 自动更新机制（增量更新 + 全量回退）
      - [ ] **Linux x86_64**
        - [ ] AppImage 格式 — 单文件分发，兼容所有主流发行版
        - [ ] Flatpak — Flathub 商店分发，沙箱隔离
        - [ ] Snap — Snapcraft 商店分发，自动更新
        - [ ] 静态链接依赖（减少运行时库冲突）
      - [ ] **macOS (Apple Silicon + Intel)**
        - [ ] 通用二进制（Universal Binary），同时支持 arm64 和 x86_64
        - [ ] DMG 磁盘映像 + 公证（Notarization）
        - [ ] Mac App Store 分发（Sandbox 适配）
        - [ ] Sparkle 自动更新框架集成
      - [ ] **跨平台构建流水线**
        - [ ] GitHub Actions 多平台并行构建（Windows/macOS/Linux）
        - [ ] 构建产物自动签名 + 打包 + 上传 Release
        - [ ] 版本号自动管理（语义化版本 + Git tag）
        - [ ] 构建产物完整性校验（SHA256 校验和）
    - [ ] **UI/UX 设计** — 全平台设计资源
  - [ ] Figma 设计稿（Vue 前端 / Mobile App / Qt Desktop）
  - [ ] 图标库 & Logo 设计
  - [ ] 产品原型 & 交互稿

### 阶段三：容器化部署 📋 规划中

#### Docker 容器化
- [ ] 每个微服务编写 Dockerfile（多阶段构建，减小镜像体积）
- [ ] Docker Compose 本地编排（一键启动全部服务）
- [ ] 镜像标签管理（语义化版本 + Git commit SHA）

#### Kubernetes 部署
- [ ] Kubernetes 资源清单（Deployment / Service / ConfigMap / Secret）
- [ ] Helm Chart 包管理
- [ ] 自动伸缩（HPA 基于 CPU/内存/QPS）
- [ ] 服务网格（Istio 流量管理 + 安全策略）

#### CI/CD 流水线
- [ ] GitHub Actions / GitLab CI 自动化构建
- [ ] 自动 proto 编译 + 多语言代码生成
- [ ] 单元测试 + 集成测试
- [ ] 镜像构建 + 推送 + 自动部署

#### 可观测性
- [ ] Prometheus 指标采集 + Grafana 仪表盘
- [ ] Jaeger 分布式链路追踪
- [ ] ELK / Loki 日志聚合
- [ ] 告警规则配置（PagerDuty / 钉钉 / 企业微信）

### 阶段四：安全加固体系 🔐 规划中

#### SecurityService 生产级部署
- [ ] 高可用部署（多副本 + 负载均衡）
- [ ] 多活密钥存储（跨区域密钥同步）
- [ ] 密钥备份与灾难恢复
- [ ] 性能优化（加密操作 < 1ms p99）

#### 传输层安全全面加固
- [ ] 所有服务间 RPC 通信启用 mTLS 双向认证
- [ ] 证书自动轮换（CertService CA + ACME 协议集成）
- [ ] 网关统一 TLS termination（支持 TLS 1.3 仅）
- [ ] 证书监控与过期预警

#### 存储层数据加密
- [ ] MySQL TDE（透明数据加密）启用
- [ ] MinIO / S3 对象存储加密（AES-256）
- [ ] 文件上传自动加密/下载自动解密
- [ ] 加密密钥与数据分离存储

#### 安全运维
- [ ] 审计日志系统（所有加解密操作、密钥操作可追溯）
- [ ] 安全合规对标（GDPR / 等保 2.0 / SOC2）
- [ ] 密钥硬安全模块（HSM）集成（AWS CloudHSM / Azure Key Vault）
- [ ] 依赖漏洞扫描（Dependabot / Trivy / Snyk）
- [ ] 代码安全审计（Rust `cargo audit` + C++ sanitizers）
- [ ] 零信任架构落地（每次请求都鉴权 + 加密 + 审计）

---

## 📬 联系方式


- 项目维护者：[jyoushitou]
- 邮箱：[xzt98948364@outlook.com]

- 博客地址：[https://jyoushitou.github.io/](https://jyoushitou.github.io/)

- 项目地址：[https://github.com/jyoushitou/WebServer](https://github.com/jyoushitou/WebServer)
- 归档仓库：[https://github.com/jyoushitou/WebSever_cpp.git](https://github.com/jyoushitou/WebSever_cpp.git)

# 📜 开源协议与致谢

### 仓库代码许可

本项目自身代码采用 **MIT License** 开源，详见 [LICENSE](./LICENSE) 文件。

```
MIT License

Copyright (c) 2025 jyoushitou

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

### 第三方组件许可

本项目使用了以下开源组件，特此致谢：

| 组件 | 许可证 | 使用方式 | 说明 |
|------|--------|---------|------|
| **Protocol Buffers** | [Apache License 2.0](https://github.com/protocolbuffers/protobuf/blob/main/LICENSE) | 代码生成 | 通过 `protoc` 编译器生成序列化代码，生成的代码可自由使用 |
| **Boost C++ Libraries** | [Boost Software License 1.0](https://www.boost.org/users/license.html) | header-only 包含 | 直接包含在 `boost/` 目录中，BSL-1.0 允许在 MIT 项目中使用 |
| **Vue 3** | [MIT License](https://github.com/vuejs/core/blob/main/LICENSE) | 前端依赖 | 独立的前端项目 |
| **Rust 依赖** (ring, rustls, tonic 等) | MIT / Apache 2.0 双许可 | 构建依赖 | 通过 Cargo 包管理器引入 |
| **Go 依赖** | BSD-style 许可 | 构建依赖 | 通过 Go Modules 引入 |

### 网络连接组件（不包含代码）

以下组件仅通过网络连接或进程调用方式使用，**不包含其源代码**：

| 组件 | 许可证 | 使用方式 |
|------|--------|---------|
| **Elasticsearch** | SSPL / Elastic License | 通过网络 API 调用 |
| **MySQL** | GPL v2 | 通过网络连接 |
| **FFmpeg** | LGPL v2.1+ | 通过进程调用 |

### 许可证兼容性说明

- **MIT + BSL-1.0**：完全兼容，Boost 库可直接包含在 MIT 项目中
- **MIT + Apache 2.0**：完全兼容，Protobuf 生成的代码可在 MIT 项目中自由使用
- **MIT + LGPL**：通过进程调用 FFmpeg 属于"合理使用"，不触发 LGPL 传染性
- **MIT + SSPL**：通过网络 API 调用 Elasticsearch 不触发许可证限制

### 版权声明

```
Protocol Buffers - Copyright (c) 2008 Google Inc. - Apache License 2.0
Boost C++ Libraries - Copyright (c) 2003-2023 Boost Contributors - BSL-1.0
Vue.js - Copyright (c) 2014-present Evan You - MIT License
```