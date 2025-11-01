# 发布说明

## 版本 1.0.0 (2025-10-27)

### 初始发布

SunshineCloud 通用自托管开发环境的首个稳定版本。

### 功能特性

#### 开发环境
- 完整的多语言开发设置
- VS Code Dev Container 集成
- Debian Bookworm 基础系统
- 具有 sudo 访问权限的用户账户 (matrix0523, Administrator)

#### AI/ML 框架支持
- Stable Diffusion WebUI (AUTOMATIC1111)
- Stable Diffusion WebUI Forge
- ComfyUI 节点化界面
- Fooocus 简化界面
- Text Generation WebUI

#### 语言运行时
- Python 3.x 支持 pip 和 conda/micromamba
- Node.js 支持 npm 和 yarn
- Java (OpenJDK 8, 11, 17, 21)
- Go 编程语言
- Ruby 支持 gem 包管理器
- PHP 支持 composer
- .NET Core 运行时

#### 开发工具
- Git 支持 Git LFS
- GitHub CLI (gh)
- Docker 和 Docker Compose
- 构建工具 (gcc, g++, make, cmake)
- 文本编辑器 (vim, nano)
- 系统监控工具

#### 包管理器
- Homebrew macOS 风格包管理
- Micromamba conda 环境管理
- 标准系统包管理器 (apt, pip, npm 等)

#### 系统服务
- SSH 服务器远程访问
- Supervisor 进程管理
- MySQL 数据库服务器
- Redis 缓存服务器
- D-Bus 系统消息传递

#### 容器功能
- 针对开发工作负载优化的 Dockerfile
- 用于服务初始化的 Docker 入口脚本
- 正确的信号处理和优雅关闭
- 资源清理和 PID 管理
- 服务健康监控

### 架构变更

#### 无头环境优化
- 移除桌面 GUI 组件 (XFCE, VNC, XRDP)
- 消除音频系统依赖 (PulseAudio)
- 移除图形应用程序和主题
- 针对服务器和开发环境进行优化

#### 容器结构
- 模块化服务配置
- 不同 AI 框架的独立容器
- 各变体间共享基础配置
- 特定环境的自定义

### 技术规格

#### 基础系统
- 操作系统: Debian 12 (Bookworm)
- 架构: x86_64, ARM64 支持
- 容器运行时: Docker 20.10+
- 资源要求: 最低 4GB 内存, 推荐 16GB

#### 网络端口
- 22: SSH 访问
- 3306: MySQL 数据库
- 6379: Redis 缓存

#### 存储布局
- 基础容器: ~5GB
- 开发工作区: /workspace
- 用户目录: /home/matrix0523, /home/Administrator
- 数据持久化: /DATA, /SunshineCloud 目录

### 安装

#### VS Code Dev Container
1. 克隆仓库
2. 在安装了 Dev Containers 扩展的 VS Code 中打开
3. 选择"在容器中重新打开"

#### 直接使用 Docker
1. 导航到 .devcontainer 目录
2. 构建: `docker build -t sunshinecloud-universal .`
3. 运行: `docker run -it --name dev-env sunshinecloud-universal`

### 配置

#### 默认用户
- matrix0523: 主要开发用户 (密码: 123456789)
- Administrator: 次要管理员用户 (密码: 123456789)

#### 环境变量
- MAMBA_ROOT_PREFIX: /opt/micromamba
- HOMEBREW_PREFIX: /SunshineCloud/Homebrew
- 区域设置: zh_CN.UTF-8 带 en_US.UTF-8 回退

### 已知问题

#### 限制
- 桌面 GUI 组件已移除（设计如此）
- 音频系统未配置（无头环境优化）
- 生产环境应更改默认密码

#### 兼容性
- 需要 Docker 20.10+ 以获得完整功能支持
- 推荐使用 VS Code Dev Containers 扩展
- GPU 支持需要 NVIDIA Container Toolkit

### 破坏性变更

无（初始发布）

### 弃用功能

无（初始发布）

### 安全说明

- 设置默认密码是为了开发便利
- 生产部署中应更改默认密码
- SSH 服务器配置用于开发用途
- 开发用户被授予 sudo 访问权限

### 迁移指南

不适用（初始发布）

### 贡献者

- SunshineCloudTech 开发团队
- 社区贡献者
- 开源项目维护者

### 致谢

特别感谢:
- AUTOMATIC1111 的 Stable Diffusion WebUI
- ComfyUI 开发团队
- Fooocus 项目贡献者
- Text Generation WebUI 维护者
- Microsoft VS Code Dev Containers 团队
- Debian 项目贡献者
- Docker 社区

---

详细的安装和使用说明，请查看 [README.md](README.md)。
错误报告和功能请求，请使用 [GitHub Issues](https://github.com/SunshineCloudTech/SunshineCloud-Universal-SelfHosted/issues)。