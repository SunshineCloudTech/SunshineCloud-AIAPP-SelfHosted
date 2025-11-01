# SunshineCloud AI应用自托管环境

一个基于Docker的AI应用自托管开发环境集合，提供多种流行的AI/ML框架的容器化部署方案。

## 项目介绍

SunshineCloud AI应用自托管环境是一个完整的容器化解决方案，专门为AI/ML开发者和爱好者设计。该项目提供了多个独立的Docker容器，每个容器都预配置了特定的AI框架和开发环境，可以快速部署到本地或服务器环境中。

## 包含的AI框架

### 图像生成框架
- **Stable Diffusion WebUI** - AUTOMATIC1111的经典WebUI界面
- **Stable Diffusion WebUI Forge** - 优化性能的WebUI变体
- **ComfyUI** - 基于节点的工作流界面
- **Fooocus** - 简化的Stable Diffusion界面

### 文本生成框架
- **Text Generation WebUI** - 大语言模型推理界面

### 开发环境特性
- VS Code Dev Container 集成
- Python 3.10+ 环境
- Node.js 开发环境
- Conda/Micromamba 包管理
- Git 版本控制
- Docker-in-Docker 支持
- NVIDIA CUDA 支持

## 项目结构

```
SunshineCloud-AIAPP-SelfHosted/
├── .devcontainer/                          # 通用开发容器配置
│   ├── devcontainer.json                  # VS Code开发容器配置
│   ├── Dockerfile                         # 主开发环境Dockerfile
│   ├── features/                          # 自定义开发环境特性
│   ├── config/                            # 服务配置文件
│   ├── environments/                      # Conda环境配置
│   ├── scripts/                           # 初始化脚本
│   └── system/                            # 系统配置
├── Install-Application/                    # 应用安装脚本
├── Install-Cuda/                          # CUDA环境安装
├── Stable-Diffusion-WebUI-SelfHosted/    # AUTOMATIC1111 WebUI容器
├── Stable-Diffusion-WebUI-Forge-SelfHosted/ # WebUI Forge容器
├── Stable-Diffusion-ComfyUI-SelfHosted/  # ComfyUI容器
├── Stable-Diffusion-Fooocus-SelfHosted/  # Fooocus容器
├── Text-Generation-WebUI-SelfHosted/     # 文本生成WebUI容器
└── scripts/                               # 通用脚本
```

## 系统要求

### 基础要求
- Docker 20.10+
- Docker Compose 2.0+
- 8GB+ 系统内存
- 50GB+ 可用磁盘空间

### GPU支持（推荐）
- NVIDIA GPU（GTX 1060 6GB 或更高）
- NVIDIA Docker Runtime
- CUDA 12.6 兼容显卡驱动

## 快速开始

### 方式一：使用VS Code开发容器

1. 克隆仓库
```bash
git clone https://github.com/SunshineCloudTech/SunshineCloud-AIAPP-SelfHosted.git
cd SunshineCloud-AIAPP-SelfHosted
```

2. 使用VS Code打开项目
   - 安装"Dev Containers"扩展
   - 打开项目文件夹
   - 选择"在容器中重新打开"

### 方式二：独立部署AI服务

选择需要的AI框架并进入对应目录：

```bash
# 部署Stable Diffusion WebUI
cd Stable-Diffusion-WebUI-SelfHosted
docker build -t sd-webui .
docker run -d --gpus all -p 7860:7860 sd-webui

# 部署ComfyUI
cd Stable-Diffusion-ComfyUI-SelfHosted
docker build -t comfyui .
docker run -d --gpus all -p 8188:8188 comfyui

# 部署Text Generation WebUI
cd Text-Generation-WebUI-SelfHosted
docker build -t textgen-webui .
docker run -d --gpus all -p 7860:7860 textgen-webui
```

## 服务端口说明

### 通用开发环境端口
- 22: SSH访问
- 8080: 文件浏览器
- 8443: Cloudflared隧道管理
- 11434: Ollama API服务

### AI服务端口
- 7860: Stable Diffusion WebUI / Text Generation WebUI
- 8188: ComfyUI界面
- 8000: Fooocus界面

## 环境配置

### 开发容器用户
- 用户名: `matrix0523`
- UID/GID: 1329
- Shell: Zsh with Oh My Zsh
- 权限: sudo访问

### Python环境
- 版本: Python 3.10+
- 包管理: pip, conda, micromamba
- 预装库: PyTorch, TensorFlow, Transformers等

### Node.js环境
- 版本: LTS
- 包管理: npm, yarn, pnpm
- 版本管理: nvm, nvs

## 各AI服务详情

### Stable Diffusion WebUI
- 基于AUTOMATIC1111的经典界面
- 支持各种Stable Diffusion模型
- 内置扩展管理和ControlNet支持
- 访问地址: http://localhost:7860

### Stable Diffusion WebUI Forge
- WebUI的性能优化版本
- 更低的显存占用
- 更快的推理速度
- 访问地址: http://localhost:7860

### ComfyUI
- 基于节点的工作流编辑器
- 高度可定制的生成流程
- 支持复杂的图像处理管道
- 访问地址: http://localhost:8188

### Fooocus
- 简化的Stable Diffusion界面
- 专注于易用性
- 适合新手用户
- 访问地址: http://localhost:8000

### Text Generation WebUI
- 大语言模型推理界面
- 支持多种模型格式
- 聊天和文本生成功能
- 访问地址: http://localhost:7860

## 开发环境特性

### 预装开发工具
- Git with LFS支持
- GitHub CLI
- Docker-in-Docker
- VS Code Server集成
- SSH服务器

### 包管理器
- Python: pip, conda, micromamba
- Node.js: npm, yarn, pnpm, nvs
- 系统包: apt, homebrew

### 机器学习包
- PyTorch
- TensorFlow  
- Transformers
- Diffusers
- OpenCV
- NumPy, Pandas
- Jupyter Lab

## 自定义配置

### 修改开发环境
1. 编辑 `.devcontainer/devcontainer.json` 调整VS Code配置
2. 修改 `.devcontainer/Dockerfile` 添加新的软件包
3. 在 `.devcontainer/features/` 目录添加自定义特性

### 配置AI服务
1. 进入对应的AI服务目录
2. 修改 `config/` 目录下的配置文件
3. 编辑 `environments/` 目录下的环境文件
4. 重新构建Docker镜像

### 环境变量配置
- 修改各服务目录下的配置文件
- 使用Docker环境变量传递配置
- 通过挂载卷持久化配置

## 使用建议

### 硬件配置建议
- 最低配置: 8GB内存, 4核CPU, GTX 1060 6GB
- 推荐配置: 16GB内存, 8核CPU, RTX 3070 8GB  
- 高性能配置: 32GB内存, 16核CPU, RTX 4090 24GB

### 存储空间
- 基础镜像: 约8GB
- AI模型存储: 20-200GB（根据需要下载的模型）
- 工作区数据: 根据项目需要

### 性能优化建议
1. 使用SSD存储提高IO性能
2. 启用GPU加速减少推理时间
3. 合理分配容器内存限制
4. 使用Docker镜像缓存加速构建

## 常见问题

### 容器启动问题
**问题**: 容器构建失败或启动失败
**解决**: 
- 检查Docker守护进程状态
- 确认系统资源充足
- 查看构建日志定位错误

### GPU相关问题  
**问题**: CUDA设备无法识别
**解决**:
- 安装NVIDIA Docker Runtime
- 检查显卡驱动版本
- 确认CUDA版本兼容性

### 内存不足问题
**问题**: AI模型加载失败或推理缓慢
**解决**:
- 增加系统内存或显存
- 使用更小的模型
- 调整batch size参数

### 网络访问问题
**问题**: 无法访问Web界面
**解决**:
- 检查端口映射配置
- 确认防火墙设置
- 验证服务启动状态

## 更新日志

查看 [RELEASE.md](RELEASE.md) 了解详细的版本更新历史。

### 最新版本 1.0.0 (2025-10-27)
- 初始发布版本
- 支持多种AI框架的容器化部署
- 完整的VS Code开发环境集成
- CUDA 12.6和多语言运行时支持

## 贡献指南

欢迎社区贡献！请遵循以下步骤：

1. Fork本仓库到您的GitHub账户
2. 创建新的功能分支 (`git checkout -b feature/amazing-feature`)
3. 提交您的更改 (`git commit -m 'Add amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 创建Pull Request

## 技术支持

- **GitHub Issues**: https://github.com/SunshineCloudTech/SunshineCloud-AIAPP-SelfHosted/issues
- **问题报告**: 请详细描述问题和复现步骤
- **功能建议**: 欢迎提出新功能需求

## 许可证

本项目基于MIT许可证开源。详情请查看 [LICENSE](LICENSE) 文件。

## 相关项目

- [AUTOMATIC1111/stable-diffusion-webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui)
- [comfyanonymous/ComfyUI](https://github.com/comfyanonymous/ComfyUI)
- [lllyasviel/Fooocus](https://github.com/lllyasviel/Fooocus)
- [oobabooga/text-generation-webui](https://github.com/oobabooga/text-generation-webui)
