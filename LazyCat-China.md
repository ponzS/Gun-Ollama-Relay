# 在懒猫微服上部署 Gun-Ollama-Relay 的指南 

兼容任何使用gunDB构建的去中心化分布式数据库应用程序

本指南提供在懒猫微服 Linux版本 系统上部署 Gun-Ollama-Relay 项目的详细步骤。该项目整合了 Gun.js 用于去中心化数据存储，以及 Ollama 用于本地运行语言模型。

前提条件

操作系统：Linux 发行版（例如 Ubuntu 20.04 或更高版本）。

硬件要求：至少 4GB 内存、2 个 CPU 核心和 20GB 可用磁盘空间（大型语言模型可能需要更多）。

软件要求：

Node.js（v16 或更高版本）和 npm。

Git 用于克隆仓库。

Ollama 用于运行语言模型。

带 bash 或兼容 shell 的终端。



### 部署步骤
1. 安装系统依赖
更新系统并安装必要工具。
```base
sudo apt update && sudo apt upgrade -y
```
```base
sudo apt install -y curl git build-essential
```

3. 安装 Node.js 和 npm
使用 nvm 安装 Node.js 和 npm 以便灵活管理版本。
```base
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```
```base
source ~/.bashrc
```
```base
nvm install 20
```
```base
node --version
```
```base
npm --version
```



5. 安装 Ollama
Ollama 是运行本地语言模型的必要组件，使用官方脚本安装。
```base
curl -fsSL https://ollama.com/install.sh | sh
```

启动 Ollama 服务并验证其运行状态。
```base
systemctl enable ollama
```
```base
systemctl start ollama
```
```base
ollama --version
```

拉取一个语言模型（例如 Llama 3.3）以供 Ollama 使用。
```base
ollama pull llama3.3
```

4. 克隆 Gun-Ollama-Relay 仓库
从 GitHub 克隆仓库。
```base
git clone https://github.com/ponzS/Gun-Ollama-Relay.git
```
```base
cd Gun-Ollama-Relay
```

6. 安装项目依赖
安装 package.json 中列出的 JavaScript 依赖。
```base
npm install
```

8. 运行应用
启动 Gun-Ollama-Relay 应用。

```base
node start.js
```
pm2 
```base
pm2 start start.js
```



7. 验证部署

 Success 终端输出以下内容代表运行成功
 ```base
=== GUN-VUE RELAY SERVER ===

Gun&Ollama Server running on http://localhost:3939
Ollama on LAN at http://192.168.1.9:3939
AXE relay enabled!
Internal URL: http://192.168.1.9:8765/
External URL: https://192.168.1.9/
Gun peer: http://192.168.1.9:8765/gun
Storage: disabled
Multicast on 233.255.255.255:8765
 ```

本地ollama API地址 ：http://localhost:3939

局域网ollama API地址 ： http://192.168.1.9:3939

GunDB数据库对等节点relay url 地址 ： http://192.168.1.9:8765/gun

是否已经开启储存 ： Storage: disabled  

如果需要储存在源代码中将store = process.env.RELAY_STORE || false,改为store = process.env.RELAY_STORE || true, 或者添加配置文件


------在TalkFlow中验证运行状态------
将数据库对等节点地址添加到relay页面中，绿色代表已建立链接

为AI创建一个身份，将ollama API地址添加到AI页面中，启用AI自动回复，在回复对象中勾选自己的身份，尝试使用自己的身份发送一条消息与AI建立沟通。

AI需要一个独立的客户端和身份运行，确保所有消息都是加密的，同时方便导出聊天记录用于您对私人AI模型的训练和其他用途。

您可以将懒猫中的TalkFlow直接作为AI的身份，然后通过iOS版本或者桌面版本与它进行沟通。或者多开应用。

8. （可选）设置为系统服务
为确保应用持续运行，创建 systemd 服务。
sudo nano /etc/systemd/system/gun-ollama-relay.service

添加以下内容，调整路径和用户名：
[Unit]
Description=Gun-Ollama-Relay 服务
After=network.target

[Service]
ExecStart=/usr/bin/npm start --prefix /path/to/Gun-Ollama-Relay
WorkingDirectory=/path/to/Gun-Ollama-Relay
Restart=always
User=你的用户名
Environment=NODE_ENV=production

[Install]
WantedBy=multi-user.target

启用并启动服务。
sudo systemctl enable gun-ollama-relay
sudo systemctl start gun-ollama-relay
sudo systemctl status gun-ollama-relay

如果希望使用pm2

npm install pm2

pm2 start start.js

9.如何在公网中开放您的ollama API和数据库对等节点

（1）.在懒猫微服商店中下载局域网转发端口

（2）.在懒猫微服商店中下载cloudflared工具

（3）.开放ollama api和数据库端口，默认ollama为3939，gunDB默认端口为8765

详情见：https://github.com/wlabby-1/cloudflared-helper


故障排查

Node.js 错误：确保使用正确的 Node.js 版本（nvm use 20）。
Ollama 无响应：检查 Ollama 服务是否运行（systemctl status ollama）以及模型是否已拉取。
Gun.js 连接问题：检查防火墙设置，确保端口 8765（或配置的端口）已开放。
日志：查看应用日志（npm start 输出或 systemd 日志，通过 journalctl -u gun-ollama-relay）。

注意事项

监控磁盘使用情况，语言模型可能占用大量空间。

如需进一步帮助，请查看仓库的问题页面：https://github.com/ponzS/Gun-Ollama-Relay/issues

感谢您的体验，我们将长期维护懒猫微服的TalkFlow版本。请随时提交改进建议
