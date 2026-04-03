# Docker
Docker是一种开源的容器化平台，允许开发者将应用程序及其依赖项打包成一个轻量级、可移植的容器中，发布到任何流行的Linux机器上，也可以实现虚拟化。Docker提供了一种简单而高效的方式来构建、部署和运行应用程序，无论是在开发环境还是生产环境中。通过使用Docker，开发者可以确保应用程序在不同环境中的一致性，并且能够更快地交付和扩展应用程序。
* 跨平台一致性：解决“在我机器上能跑”的问题，确保开发、测试、生成环境的一致性。
* 资源高效：容器直接共享主机内核，无需虚拟化整个操作系统，节省内存和CPU。
* 快速部署：秒级启动容器，支持自动化扩缩容。
* 隔离性：每个容器拥有独立的文件系统、网络和进程空间，增强安全性。
## 环境准备
### WSL2
WSL2是Windows Subsystem for Linux version 2的缩写，是微软为Windows10和Windows 11提供的一种兼容层，允许用户在Windows上运行Linux环境。WSL2引入了一个真正的Linux内核，使得性能和兼容性得到了显著提升。通过WSL2，用户可以在Windows上使用Linux命令行工具、应用程序和开发环境，而无需安装虚拟机或双系统。	
* 安装WSL2，可以通过Windows PowerShell以管理员身份运行以下命令来安装WSL2：
```powershell
wsl --install	# Windows 10 2004及以上版本已经内置WSL2安装包
```
#### 更多补充
WSL2能安装Linux分发版，可以通过以下命令来列出可用的Linux分发版并安装指定的分发版：
```powershell
wsl --list --online	# 列出可用的Linux分发版
wsl --install -d <DistributionName>	# 安装指定的Linux分发版
```

[WSL2文档](https://learn.microsoft.com/zh-cn/windows/wsl/)
### Docker-desktop
[下载地址](https://www.docker.com/)
## Image
在Docker中，`Image`即为**镜像**。只读模板，定义了容器的运行环境（如操作系统、软件配置等）。通过分层存储（Layer）优化空间和构建速度。

## Container
在Docker中，`Container`即为**容器**。轻量化的运行实例，包含应用代码、运行时环境和依赖库。基于镜像创建，与其他容器隔离，共享主力操作系统内核（比虚拟机更高效）。

## Dockerfile
Dockerfile是一个文本文件，包含了一系列指令，用于定义如何构建一个Docker镜像。

## Docker Hub
Docker Hub是一个公共的Docker镜像仓库，允许用户上传、存储和分享Docker镜像。类似与GitHub，但专门用于Docker镜像。