在懒猫微服上部署 Gun-Ollama-Relay 的指南 
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



部署步骤
1. 安装系统依赖
更新系统并安装必要工具。
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl git build-essential

2. 安装 Node.js 和 npm
使用 nvm 安装 Node.js 和 npm 以便灵活管理版本。
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
source ~/.bashrc
nvm install 20
node --version
npm --version

3. 安装 Ollama
Ollama 是运行本地语言模型的必要组件，使用官方脚本安装。
curl -fsSL https://ollama.com/install.sh | sh

启动 Ollama 服务并验证其运行状态。
systemctl enable ollama
systemctl start ollama
ollama --version

拉取一个语言模型（例如 Llama 3.3）以供 Ollama 使用。
ollama pull llama3.3

4. 克隆 Gun-Ollama-Relay 仓库
从 GitHub 克隆仓库。
git clone https://github.com/ponzS/Gun-Ollama-Relay.git
cd Gun-Ollama-Relay

5. 安装项目依赖
安装 package.json 中列出的 JavaScript 依赖。
npm install

6. 运行应用
启动 Gun-Ollama-Relay 应用。
npm start

如果仓库指定了其他启动命令（例如 node index.js），请使用该命令。查看 package.json 或 README 获取详细信息。
7. 验证部署

Gun.js：确保 Gun.js 服务器运行正常，可通过浏览器或 curl 检查默认端口（例如 http://localhost:8765）。
Ollama：通过向模型端点发送请求测试 Ollama API。

curl http://localhost:11434/api/generate -d '{"model": "llama3.3", "prompt": "你好，世界！"}'

 # Success 终端输出以下内容代表运行成功
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

