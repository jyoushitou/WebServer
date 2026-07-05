# WebServer - C++ gRPC 微服务框架

> **从零实现的 C++ HTTP 服务器 → gRPC 微服务架构演进**
>
> 用最底层的方式理解 Web 工作原理，用微服务架构承载业务扩展

![C++](https://img.shields.io/badge/C++-17-%2300599C?style=flat-square&logo=c%2B%2B)
![Rust](https://img.shields.io/badge/Rust-1.70-%23DEA584?style=flat-square&logo=rust)
![Go](https://img.shields.io/badge/Go-1.21-%2300ADD8?style=flat-square&logo=go)
![FFmpeg](https://img.shields.io/badge/FFmpeg-6.0-%23008080?style=flat-square&logo=ffmpeg)
![Vue](https://img.shields.io/badge/Vue-3-%234FC08D?style=flat-square&logo=vue.js)
![gRPC](https://img.shields.io/badge/gRPC-1.0-%234285F4?style=flat-square&logo=grpc)
![MySQL](https://img.shields.io/badge/MySQL-8-%234479A1?style=flat-square&logo=mysql)
![Protobuf](https://img.shields.io/badge/Protobuf-3.15-%23FF6C37?style=flat-square&logo=protocol-buffers)
![Elasticsearch](https://img.shields.io/badge/Elasticsearch-7-%23005571?style=flat-square&logo=elasticsearch)
![Docker](https://img.shields.io/badge/Docker-24-%232496ED?style=flat-square&logo=docker)
![Kubernetes](https://img.shields.io/badge/Kubernetes-1.28-%23326CE5?style=flat-square&logo=kubernetes)
![mTLS](https://img.shields.io/badge/mTLS-1.3-%23FF4B4B?style=flat-square&logo=letsencrypt)
![AES-256](https://img.shields.io/badge/AES--256-GCM-%2300BFA0?style=flat-square&logo=security)

---

## 📖 项目简介

本项目是一个**从零开始、不依赖任何第三方 Web 框架**的 C++ HTTP 服务器，逐步演进为完整的 gRPC 微服务架构。

> **原 C++ 单体后端已归档**：[WebSever_cpp](https://github.com/jyoushitou/WebSever_cpp.git)
>
> 归档版本为纯 C++ 实现的单体 HTTP 服务器，当前仓库为微服务架构演进版本。

### 架构演进路线

```
阶段一：单体架构（已归档）
  C++ 原生 Socket HTTP 服务器 + Vue 3 前端
  └── 手动解析 HTTP 协议、多线程处理、MySQL 直连
  └── 归档仓库：https://github.com/jyoushitou/WebSever_cpp.git

阶段二：微服务架构（进行中）
  gRPC 微服务框架 + 独立 Proto 仓库 + 服务拆分
  │
  ├── 核心框架（C++ 实现）
  │   ├── GRPCGateway         — 对外统一入口，协议转换/路由分发/限流/鉴权
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
  │       └── SecurityService — 数据加密解密/密钥管理/数字签名/mTLS 证书管理 (端口:51057) 🔐
  │
  ├── Proto 驱动开发
  │   ├── 所有跨语言通信接口由 proto 文件统一定义在 proto/source/ 目录下
  │   ├── 通过 protoc 编译生成 6 种语言的 gRPC 骨架代码
  │   │   ├── protoc (内置)     → C++   → 核心框架 + ImageService + SearchService
  │   │   ├── protoc-gen-go-grpc → Go    → ArticleService + BlogService + VideoService
  │   │   ├── protoc-gen-tonic   → Rust  → UserService + SecurityService
  │   │   ├── protoc-gen-grpc-swift → Swift → iOS App
  │   │   ├── protoc-gen-grpc-kotlin → Kotlin → Android App
  │   │   └── protoc-gen-grpc-web   → JS    → Vue 前端
  │   └── 保证跨 6 种语言服务间的接口严格一致、类型安全
  │
  └── 安全规划
      └── SecurityService 加密安全微服务（Rust），提供全链路数据加密、密钥管理、mTLS 服务间通信加密

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
  │   ├── 服务间 mTLS 双向认证（所有 gRPC 通信启用 TLS）
  │   ├── 证书自动轮换（CA 签发，7 天轮换策略）
  │   └── 网关统一 TLS  termination
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
├── GRPCGateway/                        # [子仓库] 网关 (C++) ✅ 核心
│   └── 对外统一入口，HTTP/gRPC 协议转换、路由分发、限流熔断、身份鉴权
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
│   └── 数据加密解密（AES-256-GCM/RSA-ECIES）、密钥管理、数字签名、mTLS 证书全生命周期管理
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
│   └── 用户端 Web 应用，Vue 3 + TypeScript，通过 gRPC-Web 与后端通信
│
├── mobile/                             # [子仓库] 移动端 App
│   ├── ios/                            # iOS App (Swift) — 用户端，浏览文章/博客/图片/视频
│   └── android/                        # Android App (Kotlin) — 用户端，浏览文章/博客/图片/视频
│   └── 原生移动端应用，通过 gRPC 直接连接后端服务
│
├── assets/                             # [子仓库] 设计资源
│   ├── designs/                        # UI 设计稿（Figma / Sketch）
│   ├── icons/                          # 图标库
│   ├── logos/                          # Logo 资源
│   └── mockups/                        # 产品原型 / 交互稿
│   └── 全平台设计资产仓库，统一管理前端和移动端的设计交付物
│
├── .gitmodules                         # Git 子模块配置
└── README.md                           # 本文件
```

## 🧠 架构设计

### 完整架构图

```
                    ┌──────────────────────┐    ┌───────────────────────┐    ┌──────────────────────┐
                    │      Vue 前端         │    │     iOS App (Swift)   │    │   Android App (Kotlin)│
                    │   gRPC-Web / Nginx    │    │   gRPC 客户端          │    │   gRPC 客户端          │
                    └──────────┬───────────┘    └───────────┬───────────┘    └───────────┬──────────┘
                               │                            │                            │
                               └────────────────────────────┼────────────────────────────┘
                                                           │ gRPC
                                                           ▼
┌──────────────────────────────────────────────────────────────────────────┐
│                       AdminConsole — 内部管理面板 (C++)                  │
│             管理网关 / 注册发现 / 配置中心 / 链路追踪 / 监控告警           │
│                      Admin Web UI（独立 HTML 面板）                      │
│                服务治理 · 系统监控 · 配置管理 · 健康检查                   │
└──────────────────────────────┬───────────────────────────────────────────┘
                               │ 通过 gRPC 调用各核心服务
                               ▼
┌──────────────────────────────────────────────────────────────────────────┐
│                        核心框架微服务（C++）                               │
├──────────────┬───────────────┬──────────────┬──────────────────────────┤
│ GRPCGateway  │ ServiceRegistry│ ConfigCenter │ TracingService          │
│ 协议转换/路由  │ 注册发现/负载均衡│ 配置管理/热更新│ 链路追踪/性能分析       │
├──────────────┴───────────────┴──────────────┴──────────────────────────┤
│                           MonitorService                                │
│                           监控指标/告警规则                               │
└──────────────────────────────┬───────────────────────────────────────────┘
                               │ 路由分发
                               ▼
┌──────────────────────────────────────────────────────────────────────────┐
│                    ServiceConsole — 业务管理面板 (C++)                   │
│             管理用户 / 文章 / 博客 / 图片 / 视频 / 搜索                   │
│                      Admin Web UI（独立 HTML 面板）                      │
│                  业务数据管理 · 运营统计 · 内容审核                       │
└──────────────────────────────┬───────────────────────────────────────────┘
                               │ 通过 gRPC 调用各业务服务
                               ▼
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                               业务微服务层                                            │
├────────────────┬──────────────┬──────────────┬──────────────┬──────────────────────┤
│   UserService  │   Article    │     Blog     │    Image     │       Video          │
│    (Rust)      │    (Go)      │    (Go)      │    (C++)     │       (Go)           │
│   :50052       │   :50053     │   :50054     │   :50055     │      :50056          │
│  用户认证/Token │  文章CRUD/   │  博客/评论/   │  图片处理/   │   视频转码/流媒体     │
│  权限/设备管理  │  分类/标签    │  点赞/推荐    │  缩略图/转换  │   FFmpeg 调度        │
├────────────────┴──────────────┴──────────────┴──────────────┴──────────────────────┤
│                                    │                                                │
│                                    │  ┌────────────────────────────────────────────┐│
│                                    │  │          SecurityService (Rust) 🔐          ││
│                                    │  │           端口 :51057                       ││
│                                    │  │  ┌──────────┐ ┌──────────┐ ┌──────────┐   ││
│                                    │  │  │ AES-256  │ │ Ed25519  │ │  mTLS    │   ││
│                                    │  │  │ 加/解密  │ │ 数字签名 │ │ 证书管理  │   ││
│                                    │  │  └──────────┘ └──────────┘ └──────────┘   ││
│                                    │  └────────────────────────────────────────────┘│
│                                    │                                                │
│                                    ▼                                                │
│                    ┌──────────────────────────────┐                                │
│                    │       SearchService (C++)      │                              │
│                    │        端口 :50057              │                              │
│                    │  全文索引 / 搜索排序 / 搜索建议   │                              │
│                    └──────────────┬─────────────────┘                              │
│                                   │                                                │
│                                   ▼                                                │
│                    ┌──────────────────────────────┐                                │
│                    │      Elasticsearch (搜索引擎)   │                               │
│                    │         端口 :9200             │                               │
│                    └──────────────────────────────┘                                │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

### 核心框架组件（C++ 实现）

| 微服务 | 优先级 | 职责 | 说明 |
|--------|--------|------|------|
| **GRPCGateway** | ✅ 核心 | 对外统一入口 | 协议转换、路由分发、限流控制、鉴权验证 |
| **ServiceRegistry** | ✅ 核心 | 服务注册发现 | 动态路由、负载均衡、健康检查、服务上下线 |
| **ConfigCenter** | ✅ 核心 | 配置中心 | 统一配置管理、热更新、配置版本控制 |
| **TracingService** | ✅ 核心 | 链路追踪 | 请求全链路跟踪、性能分析、故障排查 |
| **MonitorService** | ✅ 核心 | 监控告警 | 指标采集、告警规则、可视化面板 |
| **AdminConsole** | ✅ 核心 | 内部管理面板 | 管理核心框架服务（网关/注册发现/配置中心/链路追踪/监控告警）的独立 Web UI |
| **ServiceConsole** | ✅ 核心 | 业务管理面板 | 管理业务服务（用户/文章/博客/图片/视频/搜索）的独立 Web UI |

### 业务微服务（多语言）

| 微服务 | 语言 | 端口 | 职责 |
|--------|------|------|------|
| UserService | Rust | 50052 | 用户注册/登录、Token 管理、权限控制 |
| ArticleService | Go | 50053 | 文章 CRUD、分类管理、标签管理 |
| BlogService | Go | 50054 | 博客管理、评论系统、点赞收藏 |
| ImageService | C++ | 50055 | 图片像素级处理、缩略图生成、格式转换 |
| VideoService | Go | 50056 | 视频上传、转码任务调度（FFmpeg）、流媒体分发 |
| SearchService | C++ | 50057 | 全文索引、搜索排序、搜索建议 |

### 安全微服务（Rust 实现 🔐）

| 微服务 | 语言 | 端口 | 职责 | 核心算法/协议 |
|--------|------|------|------|-------------|
| **SecurityService** | Rust | 51057 | 数据加密/解密、密钥管理、数字签名、mTLS 证书管理 | AES-256-GCM / RSA-OAEP / ECIES / Ed25519 / ECDSA / SHA-256 / SHA-3 |

#### 端口规划说明

| 端口范围 | 服务类型 | 说明 |
|---------|---------|------|
| 50051–50057 | 业务微服务 | 网关 + 6 个业务服务（User/Article/Blog/Image/Video/Search） |
| 51051–51057 | 核心框架 + 安全 | 注册发现/配置中心/链路追踪/监控告警 + 管理面板 + SecurityService |
| 60907 | 前端 | Vue 3 开发服务器 |

---

## 📋 微服务简介

> 本章节对架构中 **16 个微服务**逐一进行详细介绍，涵盖语言选择、核心职责、技术架构和设计理念。

---

### 🏛️ 核心框架层（C++）

#### 🔷 GRPCGateway — 网关服务

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 50051 |
| **优先级** | ✅ 核心 |
| **代码仓库** | `GRPCGateway/` |

GRPCGateway 是整个微服务架构的**流量枢纽**，所有来自 Vue 前端、iOS App、Android App 的外部请求都必须经过它。它的核心职责包括：**协议转换**——将外部的 HTTP/1.1、HTTP/2 请求转换为内部的 gRPC 调用，屏蔽后端多语言异构性；**路由分发**——根据请求头中的服务名和方法名，将请求精准路由到对应的微服务实例；**限流熔断**——基于令牌桶算法对每个客户端 IP 和 API 路径做限流，超出阈值时直接返回 429；**身份鉴权**——拦截所有请求，调用 UserService.VerifyToken 校验 Token 有效性，将用户身份注入 gRPC 上下文元数据。网关采用 C++ 实现，基于 epoll 事件驱动模型，单机可支撑数万并发连接，是微服务架构的第一道防线。

---

#### 🔷 ServiceRegistry — 服务注册发现

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 51051 |
| **优先级** | ✅ 核心 |
| **代码仓库** | `ServiceRegistry/` |

ServiceRegistry 维护着整个集群的**动态服务注册表**，是微服务间通信的"通讯录"。每个微服务启动时通过 `Register` RPC 注册自己的实例信息（服务名、主机、端口、元数据标签），随后每隔 TTL 秒发送一次心跳续约。网关和其他服务通过 `Discover` RPC 获取目标服务的可用实例列表，支持加权轮询、最小连接数等负载均衡策略。当某个实例连续多次心跳超时，服务注册表自动将其标记为下线并从可用列表剔除。`Watch` 接口提供服务端流推送，订阅者能实时感知服务上下线事件。这种方式实现了服务的**动态发现**和**优雅上下线**，无需硬编码任何地址。

---

#### 🔷 ConfigCenter — 配置中心

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 51052 |
| **优先级** | ✅ 核心 |
| **代码仓库** | `ConfigCenter/` |

ConfigCenter 解决微服务架构中配置管理的核心痛点：**配置散落、变更需重启、版本难追溯**。所有微服务的配置项集中存储在 ConfigCenter 中，每个配置项包含键值对、版本号和更新时间戳。服务启动时调用 `GetConfig` 拉取全部配置，运行期间通过 `WatchConfig` 订阅配置变更事件——当运维人员在 AdminConsole 上修改某个配置后，ConfigCenter 实时推送新配置到所有订阅实例，实现**热更新**无需重启。`GetConfigHistory` 提供配置变更的完整审计轨迹，方便回滚到任意历史版本。配置按服务名和配置键两层命名空间隔离，避免不同服务间的配置冲突。

---

#### 🔷 TracingService — 链路追踪

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 51053 |
| **优先级** | ✅ 核心 |
| **代码仓库** | `TracingService/` |

TracingService 实现了基于 **Google Dapper** 论文的分布式链路追踪系统。当一个请求跨越多个微服务时（例如：网关 → UserService → ArticleService → SecurityService），每个服务处理该请求时都会生成一个 Span（跨度），记录服务名、方法名、开始时间、结束时间、状态和自定义标签。所有 Span 通过全局唯一的 TraceID 串联起来，上报到 TracingService。运维人员在 AdminConsole 上可以根据 TraceID 精确检索某次请求的完整调用链，也可以按服务名、方法名、耗时范围搜索慢请求。`GetServiceMap` 接口通过分析 Span 中的父子关系，自动构建**服务依赖拓扑图**，直观展示服务间的调用关系和流量大小，是排查故障和发现异常依赖的利器。

---

#### 🔷 MonitorService — 监控告警

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 51054 |
| **优先级** | ✅ 核心 |
| **代码仓库** | `MonitorService/` |

MonitorService 是微服务架构的 **"体检中心"**，负责实时采集所有服务的健康指标。每个微服务（包括 MonitorService 自身）通过 `ReportMetric` 定期上报 CPU 使用率、内存占用、QPS、P50/P95/P99 延迟、错误率等指标。`SetAlertRule` 允许运维人员配置告警规则，例如："当错误率连续 5 分钟超过 5% 时触发告警"。告警可通过邮件、钉钉、企业微信等多种渠道通知。`GetServiceHealth` 提供一键健康检查，快速了解整个集群的运行状态。所有指标数据按时间序列存储，支持按分钟、小时、日维度聚合查询，为 Grafana 仪表盘提供数据源，实现可视化的运维监控。

---

### 📊 管理面板层（C++）

#### 🟦 AdminConsole — 内部管理面板

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 51055 |
| **优先级** | ✅ 核心 |
| **代码仓库** | `AdminConsole/` |

AdminConsole 是面向**运维人员**的统一管理控制台，采用独立 HTML + JS 前端架构，通过 gRPC 直连各核心服务（不经过网关转发），确保管理通道高可用。它提供五大管理模块：**服务治理**——查看 GRPCGateway 路由规则和限流配置，管理 ServiceRegistry 中的实例上下线；**系统监控**——以仪表盘形式实时展示 CPU/内存/QPS/延迟趋势图；**配置管理**——可视化编辑 ConfigCenter 中的配置项并推送热更新；**链路追踪**——按 TraceID 搜索调用链，查看 Span 详情和瀑布图；**告警管理**——设置告警规则、查看告警历史和处理记录。所有管理操作均对接 UserService 进行管理员身份认证，并记录详细审计日志。

---

#### 🟦 ServiceConsole — 业务管理面板

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 51056 |
| **优先级** | ✅ 核心 |
| **代码仓库** | `ServiceConsole/` |

ServiceConsole 是面向**运营人员**的业务管理控制台，提供对业务数据的可视化管理和内容审核能力。核心功能包括：**用户管理**——查看用户列表、分配角色权限、封禁/解封违规账号；**内容审核**——对文章、博客、图片、视频进行合规审核，支持审核队列、批量操作和审核历史追溯；**运营统计**——展示用户增长趋势、内容产出量、文章阅读量、视频播放量、互动数据等业务指标图表。与 AdminConsole 类似，它也是独立 Web UI 架构，通过 gRPC 直连各业务微服务，管理操作同步记录审计日志。

---

### 🔐 安全层（Rust）

#### 🟥 SecurityService — 加密安全服务

| 属性 | 说明 |
|------|------|
| **语言** | Rust（基于 `ring`、`rustls`、`tonic` 加密库） |
| **端口** | 51057 |
| **优先级** | ✅ 核心 |
| **代码仓库** | `SecurityService/` |

SecurityService 是整个微服务架构的**安全基石**，提供纵深防御模型中的第一层（业务层）加密能力。选择 Rust 实现是经过深思熟虑的——安全服务是所有微服务中最不能出内存安全问题的环节，Rust 的所有权系统和零成本抽象保证了加密算法的高性能执行，同时彻底杜绝了缓冲区溢出、空指针解引用等常见内存漏洞。它的核心能力分为四大模块：

**数据加密**：提供 AES-256-GCM 对称加密和 RSA-OAEP/ECIES 非对称加密，每次加密生成随机 IV，支持附加认证数据（AAD），确保密文的机密性和完整性。业务服务需要加密敏感字段（如手机号、身份证）时，只需调用 `Encrypt` RPC 即可。

**数字签名**：支持 Ed25519（性能优先）和 ECDSA P-256（兼容优先）两种签名算法，确保数据的来源可信和不可否认性。签名密钥每 90 天自动轮换。

**密钥管理**：统一管理所有加密密钥的全生命周期——密钥生成、安全存储、版本化、自动轮换（加密密钥 30 天、签名密钥 90 天）、到期吊销。旧密钥保留 180 天用于解密历史数据。

**mTLS 证书管理**：作为内部 CA，为所有微服务签发短生命周期（7 天）的 mTLS 证书，到期前 24 小时自动续期。这为阶段四的全网 mTLS 双向认证奠定了技术基础。

---

### 🟢 业务服务层（多语言）

#### UserService — 用户服务（Rust）

| 属性 | 说明 |
|------|------|
| **语言** | Rust |
| **端口** | 50052 |
| **类型** | 安全敏感型 |
| **代码仓库** | `UserService/` |

UserService 是平台的**用户认证中心**，负责所有与用户身份相关的操作。选择 Rust 是因为用户数据是安全敏感度最高的业务数据——密码哈希、Token 签发、权限校验等操作不容半点内存安全闪失。核心功能包括：用户注册时使用 bcrypt 加盐哈希存储密码；登录成功后签发 JWT Token（含用户 ID、权限角色、过期时间）；`VerifyToken` RPC 被 GRPCGateway 调用以验证每个请求的 Token 有效性；支持 RBAC 权限模型，每个用户可分配多个角色，每个角色拥有不同的资源操作权限；多设备管理功能允许用户查看和移除自己的登录设备。所有用户敏感数据（如手机号、邮箱）在存储前都通过 SecurityService 进行字段级加密。

---

#### ArticleService — 文章服务（Go）

| 属性 | 说明 |
|------|------|
| **语言** | Go |
| **端口** | 50053 |
| **类型** | I/O 密集型 CRUD |
| **代码仓库** | `ArticleService/` |

ArticleService 是**内容管理系统（CMS）的核心**，负责文章的完整生命周期管理。选择 Go 是因为文章服务本质上是 I/O 密集型 CRUD 操作——大量数据库读写、缓存查询，Go 的 goroutine 协程模型可以轻松处理数百个并发数据库连接而不产生线程切换开销。文章支持多级分类和标签两种维度的组织方式，文章列表支持按分类、标签、作者、时间范围进行过滤和分页查询。每篇文章的 view_count 通过 Redis 原子自增实现高并发计数。文章内容支持 Markdown 格式，HTML 渲染在服务端完成以确保安全。服务内集成 SecurityService 客户端 SDK，在存储和返回用户敏感数据时自动调用加解密接口。

---

#### BlogService — 博客服务（Go）

| 属性 | 说明 |
|------|------|
| **语言** | Go |
| **端口** | 50054 |
| **类型** | I/O 密集型 CRUD + 社交互动 |
| **代码仓库** | `BlogService/` |

BlogService 提供**社交化博客平台**能力，在文章发布基础上增加了用户互动功能。Go 的 goroutine 在处理大量并发互动请求（评论提交、点赞操作）时具有天然优势——每个互动请求可以分配一个轻量级 goroutine 处理，内存占用远低于线程。核心功能包括：博客文章 CRUD；嵌套评论系统（支持楼中楼回复、评论审核）；点赞/取消点赞（使用 Redis Set 防止重复点赞）；基于用户行为（阅读历史、点赞标签、关注的作者）的个性化博客推荐算法。评论和点赞数据通过消息队列异步写入数据库，避免高并发下的数据库写冲突。敏感操作（如删除评论）需要通过 UserService 验证操作者权限。

---

#### ImageService — 图片服务（C++）

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 50055 |
| **类型** | 计算密集型 |
| **代码仓库** | `ImageService/` |

ImageService 是**高性能图片处理引擎**，选择 C++ 是因为图片处理是典型的计算密集型任务——像素级操作、编解码、缩放滤波等算法需要极致的 CPU 利用率。C++ 允许直接调用 SIMD 指令集（AVX2/NEON）和 GPU 加速库，性能远超 Go 和 Rust 的通用方案。核心功能包括：客户端流式上传（分块传输，支持断点续传）；原图存储到对象存储后，自动生成多尺寸缩略图（128×128、256×256、512×512、1024×1024）；格式转换支持 JPEG、PNG、WebP、AVIF，根据客户端 Accept 头自动选择最优格式；支持批量处理任务，利用 C++ 线程池并行处理，充分发挥多核 CPU 性能。每个图片处理流水线使用独立的沙箱进程，防止恶意图片导致服务崩溃。

---

#### VideoService — 视频服务（Go）

| 属性 | 说明 |
|------|------|
| **语言** | Go |
| **端口** | 50056 |
| **类型** | 任务调度 I/O 密集型 |
| **代码仓库** | `VideoService/` |

VideoService 是**视频处理调度中心**，Go 的并发模型在这里发挥最大价值——视频转码是典型的异步任务调度场景。用户上传视频后，服务不阻塞等待转码完成，而是立即返回 video_id 和转码任务 job_id，后续通过 `GetTranscodingStatus` 轮询进度。核心流程：视频文件通过客户端流式上传到临时存储 → 启动 FFmpeg 转码任务（根据配置的目标格式和分辨率生成多个输出文件）→ 将转码完成的切片上传到 CDN 源站 → HLS 视频切片分发（生成 .m3u8 索引文件和 .ts 切片文件）。Go 的 goroutine 管理 FFmpeg 子进程池，通过 Channel 通信协调任务队列，避免资源竞争。转码进度信息通过 Redis Pub/Sub 实时推送给前端。视频元数据和敏感信息（如上传者 IP）存储前调用 SecurityService 加密。

---

#### SearchService — 搜索服务（C++）

| 属性 | 说明 |
|------|------|
| **语言** | C++17 |
| **端口** | 50057 |
| **类型** | 计算密集型 + ES 集成 |
| **代码仓库** | `SearchService/` |

SearchService 是**全文搜索引擎**的上层封装，选择 C++ 实现查询解析层和排序算法层，以获取最优的查询性能。底层使用 Elasticsearch 作为倒排索引存储引擎，C++ 层负责查询语句的解析优化、分布式聚合查询的协调、以及自定义排序评分算法的执行。核心功能：**全文搜索**——支持多字段检索（标题、内容、标签）、布尔查询、短语匹配、模糊查询；**搜索排序**——基于 TF-IDF + 业务权重（如文章热度、发布时间）的混合排序模型；**搜索建议**——基于前缀匹配的热门搜索词补全，毫秒级响应；**索引管理**——支持按类型重建索引、增量同步、索引健康监控。搜索性能是 C++ 层的核心竞争力——查询解析和排序计算在 C++ 层完成，ES 仅负责索引检索，减少了网络开销和 ES 集群的计算压力。

---

### 🖥️ 前端 & 客户端

#### Vue 3 前端

| 属性 | 说明 |
|------|------|
| **技术栈** | Vue 3 + TypeScript + gRPC-Web |
| **端口** | 60907 |
| **代码仓库** | `vue/` |

Vue 3 前端是用户端 Web 应用，采用 Composition API + TypeScript 构建。与传统 RESTful 架构不同，前端通过 **gRPC-Web** 直接与 GRPCGateway 通信，二进制 Protobuf 编码比 JSON 快 2~5 倍，且通过 `.proto` 文件生成的 TypeScript 类型定义保证了前后端接口的严格一致性。前端负责提供文章浏览和搜索、博客阅读和评论互动、图片和视频展示、用户登录注册等完整用户体验。使用 Vite 作为构建工具，开发服务器热更新速度毫秒级。

#### iOS App（Swift）

| 属性 | 说明 |
|------|------|
| **技术栈** | Swift + gRPC Swift |
| **代码仓库** | `mobile/ios/` |

iOS 原生 App 使用 Swift 开发，集成 gRPC Swift 客户端库，通过 proto 文件编译生成的 Swift 代码直接调用后端微服务。核心页面包括内容信息流浏览、文章/博客详情阅读、图片画廊、视频播放、用户登录注册和个人中心。运用 SwiftUI 声明式 UI 框架实现响应式界面。

#### Android App（Kotlin）

| 属性 | 说明 |
|------|------|
| **技术栈** | Kotlin + gRPC Kotlin |
| **代码仓库** | `mobile/android/` |

Android 原生 App 使用 Kotlin + Jetpack Compose 开发，通过 gRPC Kotlin 协程客户端与后端通信，利用 Kotlin Flow 实现数据的响应式更新。功能覆盖与 iOS App 一致，确保跨平台用户体验的统一性。

---

## 🔌 核心框架微服务接口

### GRPCGateway — 网关服务（C++，端口:50051）

对外统一入口，负责 HTTP/gRPC 协议转换、路由分发、限流控制、鉴权验证。

| RPC | 请求 | 响应 | 说明 |
|-----|------|------|------|
| `RouteRequest` | service_name, method_name, payload, metadata | status_code, data, error_message | 路由请求到后端微服务 |
| `HttpToGrpc` | method, path, headers, body, query_params | status_code, headers, body | HTTP 协议转 gRPC |
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

全链路安全微服务，提供敏感数据端到端加密、密钥生命周期管理、数字签名验签、服务间 mTLS 双向认证管理。

> **为什么用 Rust？** 安全服务是所有微服务中最不能出内存安全问题的环节。Rust 的所有权系统和零成本抽象保证了加密算法的高性能执行，同时彻底杜绝了缓冲区溢出、空指针解引用等内存安全漏洞。对比 C++，Rust 的 trait 系统让加密算法的组合更安全、更可预测。

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

### mTLS 证书管理 — 服务间双向认证

| RPC | 请求 | 响应 | 说明 |
|-----|------|------|------|
| `GetCertificate` | service_name | certificate, private_key, ca_cert, expires_at | 获取 mTLS 证书（含 CA 证书链） |
| `RenewCertificate` | service_name | certificate, private_key, ca_cert, expires_at | 续期 mTLS 证书（到期前自动触发） |
| `ValidateCertificate` | certificate, service_name | valid, san, expires_at, issuer | 验证证书有效性（检查 SAN、有效期、CA 签名） |

**证书管理机制**：
- 内部 CA 签发，所有微服务信任同一根 CA
- 证书有效期：7 天（短生命周期减少泄露风险）
- 自动续期：到期前 24 小时自动发起续期请求
- 吊销支持：紧急情况下可吊销任意服务证书

### 加密层级设计（纵深防御模型）

```
数据分层加密模型：
┌─────────────────────────────────────────────────────────────────────┐
│  第一层：业务层 — SecurityService 字段级加密                        │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │  适用场景：用户手机号、身份证、银行卡、医疗记录等 PII 数据      │   │
│  │  加密方式：AES-256-GCM，每次加密生成随机 IV                   │   │
│  │  密钥策略：每 30 天自动轮换，旧密钥保留 180 天用于解密          │   │
│  │  调用方式：业务服务通过 gRPC 调用 SecurityService.Encrypt()    │   │
│  └─────────────────────────────────────────────────────────────┘   │
├─────────────────────────────────────────────────────────────────────┤
│  第二层：传输层 — 服务间 mTLS 双向认证                             │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │  适用场景：所有微服务间的 gRPC 通信                           │   │
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
│  前端     │───▶│  GRPCGateway │───▶│  业务服务(Go)     │───▶│  MySQL   │
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
| MySQL | 8.0+ | 数据库 |
| gRPC | 1.40+ | 微服务通信框架 |
| Protobuf | 3.15+ | 序列化协议 |
| protoc-gen-go-grpc | 1.2+ | Go gRPC 代码生成插件 |
| protoc-gen-tonic | 0.3+ | Rust gRPC 代码生成插件 |
| protoc-gen-grpc-swift | 1.0+ | Swift gRPC 代码生成插件 |
| protoc-gen-grpc-kotlin | 1.3+ | Kotlin gRPC 代码生成插件 |
| protoc-gen-grpc-web | 1.4+ | JavaScript gRPC-Web 代码生成插件 |
| Rust | 1.70+ | UserService（安全敏感认证服务）、SecurityService（加密安全服务，依赖 `ring`/`rustls`/`tonic` 等加密库） |
| Go | 1.21+ | ArticleService、BlogService、VideoService（I/O 密集型服务） |
| Node.js | 18+ | 前端构建 |

### 1️⃣ 克隆仓库

```bash
git clone --recursive https://github.com/jyoushitou/WebServer.git
cd WebServer
git submodule update --init --recursive
```

### 2️⃣ 检查前置工具

```bash
# 检查 protoc 及各语言插件是否安装
protoc --version                                                     # 需要 3.15+
which grpc_cpp_plugin                                               # C++ gRPC 插件
protoc-gen-go-grpc --version || echo "需要安装 protoc-gen-go-grpc"   # Go 插件
protoc-gen-tonic --version || echo "需要安装 protoc-gen-tonic"      # Rust 插件
protoc-gen-grpc-web --version || echo "需要安装 protoc-gen-grpc-web" # JS 插件

# 检查 Rust 工具链（SecurityService 需要）
rustc --version                                  # 需要 1.70+
cargo install --list | grep protoc-gen-tonic    # Rust protoc 插件
cargo install --list | grep ring                 # 加密库（AES-256-GCM 等）
```

### 2️⃣ 编译 Proto 库（生成多语言代码）

#### 什么是 Proto 驱动开发？

本项目所有微服务之间的通信接口，**全部由 `.proto` 文件统一定义**，然后通过 `protoc` 编译器生成各语言的 gRPC 骨架代码。这种方式的好处是：

1. **接口即契约** — 一个 `.proto` 文件同时约束 6 种语言，保证接口完全一致
2. **类型安全** — 编译时检查类型，避免运行时序列化错误
3. **零运行时开销** — gRPC 使用 Protobuf 二进制序列化，比 JSON 快 10-100 倍
4. **多语言原生支持** — 每种语言生成对应风格的代码（C++ 类、Go struct、Rust struct 等）

#### 目录结构说明

```
proto/
├── source/                          # Proto 源文件（接口定义）
│   ├── common/                      # 公共类型、枚举、错误码
│   │   └── common.proto            # RequestId、Pagination、ErrorCode
│   ├── gateway/                     # 网关服务接口
│   │   └── gateway.proto           # RouteRequest、HttpToGrpc、RateLimit
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
│   ├── frontend/                    # 前端服务接口（gRPC-Web）
│   ├── admin_console/               # 内部管理面板接口
│   └── service_console/             # 业务管理面板接口
└── build/                           # 编译输出
    └── generated/
        ├── cpp/                     # C++ 生成代码
        ├── go/                      # Go 生成代码
        ├── rust/                    # Rust 生成代码
        ├── swift/                   # Swift 生成代码
        ├── kotlin/                  # Kotlin 生成代码
        └── js/                      # JavaScript 生成代码（gRPC-Web）
```

#### 生成 C++ 代码（核心框架）

```bash
cd proto/source && mkdir -p ../build && cd ../build
cmake ../source && make -j$(nproc)
```

#### 生成各语言 gRPC 代码

每种语言需使用对应的 protoc 插件编译 `.proto` 文件到各自的代码目录：

| 目标语言 | protoc 插件 | 安装命令 | 输出目录 | 使用者 |
|---------|------------|---------|---------|-------|
| **C++** | `protoc` 内置 grpc_cpp_plugin | `apt install protobuf-compiler-grpc` | `build/generated/cpp/` | GRPCGateway、ServiceRegistry、ConfigCenter、TracingService、MonitorService、AdminConsole、ServiceConsole、ImageService、SearchService |
| **Go** | `protoc-gen-go-grpc` | `go install google.golang.org/protobuf/cmd/protoc-gen-go@latest` | `build/generated/go/` | ArticleService、BlogService、VideoService |
| **Rust** | `protoc-gen-tonic` | `cargo install protoc-gen-tonic` | `build/generated/rust/` | UserService、SecurityService 🔐 |
| **Swift** | `protoc-gen-grpc-swift` | `brew install grpc-swift` | `build/generated/swift/` | iOS App |
| **Kotlin** | `protoc-gen-grpc-kotlin` | 使用 Gradle plugin `com.google.protobuf` | `build/generated/kotlin/` | Android App |
| **JavaScript** | `protoc-gen-grpc-web` | `npm install -g protoc-gen-grpc-web` | `build/generated/js/` | Vue 前端（gRPC-Web） |

一键生成所有语言代码示例：

```bash
cd proto
PROTO_DIR=source
OUT_DIR=build/generated

# 查找所有 .proto 文件
PROTO_FILES=$(find $PROTO_DIR -name "*.proto")

# ─── C++ ─────────────────────────────────────
protoc --cpp_out=$OUT_DIR/cpp --grpc_out=$OUT_DIR/cpp \
  --plugin=protoc-gen-grpc=`which grpc_cpp_plugin` \
  -I $PROTO_DIR $PROTO_FILES
echo "✅ C++ 代码生成完成"

# ─── Go ───────────────────────────────────────
protoc --go_out=$OUT_DIR/go --go-grpc_out=$OUT_DIR/go \
  -I $PROTO_DIR $PROTO_FILES
echo "✅ Go 代码生成完成"

# ─── Rust ─────────────────────────────────────
protoc --rust_out=$OUT_DIR/rust --tonic_out=$OUT_DIR/rust \
  -I $PROTO_DIR $PROTO_FILES
echo "✅ Rust 代码生成完成"

# ─── Swift ────────────────────────────────────
protoc --swift_out=$OUT_DIR/swift --grpc-swift_out=$OUT_DIR/swift \
  -I $PROTO_DIR $PROTO_FILES
echo "✅ Swift 代码生成完成"

# ─── Kotlin ───────────────────────────────────
protoc --kotlin_out=$OUT_DIR/kotlin --grpc-kotlin_out=$OUT_DIR/kotlin \
  -I $PROTO_DIR $PROTO_FILES
echo "✅ Kotlin 代码生成完成"

# ─── JavaScript (gRPC-Web) ────────────────────
protoc --js_out=import_style=commonjs:$OUT_DIR/js \
  --grpc-web_out=import_style=commonjs,mode=grpcwebtext:$OUT_DIR/js \
  -I $PROTO_DIR $PROTO_FILES
echo "✅ JavaScript 代码生成完成"
```

> **注意**：生成代码通常不提交到 Git 仓库，而是在 CI/CD 构建流程中自动生成，或通过 `git submodule` 方式在各微服务仓库中独立编译。

#### security.proto 示例（SecurityService 接口定义片段）

```protobuf proto/source/security/security.proto
syntax = "proto3";
package security;
option go_package = "proto/build/generated/go/security";

// 加密安全服务 — 全链路加密、密钥管理、数字签名、mTLS 证书
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

  // ── 证书管理 ──
  rpc GetCertificate(GetCertificateRequest) returns (GetCertificateResponse);
  rpc RenewCertificate(RenewCertificateRequest) returns (RenewCertificateResponse);
  rpc ValidateCertificate(ValidateCertificateRequest) returns (ValidateCertificateResponse);

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
cd GRPCGateway && mkdir build && cd build
cmake .. && cmake --build . && ./GRPCGateway

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
| GRPCGateway | C++ | 50051 | 核心框架 | 网关层 |
| ServiceRegistry | C++ | 51051 | 核心框架 | 基础设施层 |
| ConfigCenter | C++ | 51052 | 核心框架 | 基础设施层 |
| TracingService | C++ | 51053 | 核心框架 | 可观测性层 |
| MonitorService | C++ | 51054 | 核心框架 | 可观测性层 |
| AdminConsole | C++ | 51055 | 核心框架 | 管理面板层 |
| ServiceConsole | C++ | 51056 | 核心框架 | 管理面板层 |
| SecurityService | Rust | 51057 | 安全服务 🔐 | 安全层 |
| UserService | Rust | 50052 | 业务服务 | 业务层 |
| ArticleService | Go | 50053 | 业务服务 | 业务层 |
| BlogService | Go | 50054 | 业务服务 | 业务层 |
| ImageService | C++ | 50055 | 业务服务 | 业务层 |
| VideoService | Go | 50056 | 业务服务 | 业务层 |
| SearchService | C++ | 50057 | 业务服务 | 业务层 |
| Vue 前端 | JS | 60907 | 前端 | 前端层 |
| Elasticsearch | Java | 9200 | 搜索引擎 | 数据层 |

---

## 📋 开发计划

### 阶段一：单体架构 ✅ 已归档
- [x] C++ 原生 Socket HTTP 服务器
- [x] HTTP 协议手动解析、多线程并发处理
- [x] MySQL 数据库直连、Vue 3 前端界面
- [x] 用户认证系统（Token）、异步任务处理
- [x] 归档仓库：https://github.com/jyoushitou/WebSever_cpp.git

### 阶段二：微服务架构 🔄 进行中

#### Proto 仓库搭建 ✅ 已完成
- [x] Proto 独立仓库搭建（proto/ 子仓库）
- [x] Proto 目录结构规划（common/gateway/registry/config/tracing/monitor/user/article/blog/image/video/search/frontend/admin_console/service_console）
- [x] 公共类型定义（RequestId、Pagination、ErrorCode 等通用 message）
- [x] **security/** 目录规划 — 加密安全服务接口（新增）

#### Proto 多语言编译 ✅ 已完成
- [x] CMake 构建脚本 — 编译 C++ gRPC 代码
- [ ] protoc 编译命令 — 支持 C++ / Go / Rust / Swift / Kotlin / JS 六种语言
- [x] 各语言 protoc 插件安装说明（protoc-gen-go-grpc / protoc-gen-tonic / protoc-gen-grpc-swift / protoc-gen-grpc-kotlin / protoc-gen-grpc-web）
- [x] 一键编译脚本模板

#### AdminConsole 内部管理面板 (C++)
- [ ] **Admin Web UI 前端** — 独立 HTML + JS 管理面板
  - [ ] 服务治理页面 — 查看所有核心服务状态、启停控制
  - [ ] 系统监控仪表盘 — CPU/内存/QPS/延迟实时展示
  - [ ] 配置管理页面 — 查看/编辑/推送配置
  - [ ] 健康检查页面 — 各服务健康状态概览
- [ ] **后端对接**
  - [ ] 对接 GRPCGateway 网关管理 — 查看路由规则、限流配置
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
  - [ ] GetCertificate/RenewCertificate/ValidateCertificate — mTLS 证书管理接口
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
  - [ ] 对接 GRPCGateway 网关 — 敏感字段自动加密/解密
  - [ ] 对接 AdminConsole — 密钥管理面板、证书状态查看
  - [ ] 对接 ServiceRegistry — 安全服务注册发现
  - [ ] 所有业务服务集成 SecurityService 客户端 SDK

#### 业务微服务
- [ ] **UserService**（Rust — 安全敏感，内存安全优势）
  - [ ] 用户注册/登录、Token 签发
  - [ ] Token 验证（对接 GRPCGateway 鉴权中间件）
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
- [ ] **Vue 前端** gRPC-Web 接入
  - [ ] 文章浏览/搜索页面
  - [ ] 博客/评论互动页面
  - [ ] 图片/视频展示页面
  - [ ] 用户登录/注册页面
- [ ] **iOS App**（Swift）
  - [ ] gRPC 客户端集成
  - [ ] 文章/博客/图片/视频浏览
  - [ ] 用户登录/注册
- [ ] **Android App**（Kotlin）
  - [ ] gRPC 客户端集成
  - [ ] 文章/博客/图片/视频浏览
  - [ ] 用户登录/注册
- [ ] **UI/UX 设计** — 全平台设计资源
  - [ ] Figma 设计稿（Vue 前端 / Mobile App）
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
- [ ] 所有服务间 gRPC 通信启用 mTLS 双向认证
- [ ] 证书自动轮换（CA + ACME 协议集成）
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
- 项目地址：[https://github.com/jyoushitou/WebServer](https://github.com/jyoushitou/WebServer)
- 归档仓库：[https://github.com/jyoushitou/WebSever_cpp.git](https://github.com/jyoushitou/WebSever_cpp.git)