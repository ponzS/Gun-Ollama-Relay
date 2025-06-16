# Guide to deploy Gun-Ollama-Relay on the lazy cat microserver

Compatible with any decentralized distributed database application built with gunDB

This guide provides detailed steps for deploying the Gun-Ollama-Relay project on the Lazy Cat Microserver Linux version system. The project integrates Gun.js for decentralized data storage and Ollama for local running language models.

Prerequisite

Operating system: Linux distribution (such as Ubuntu 20.04 or later).

Hardware requirements: at least 4GB of memory, 2 CPU cores and 20GB of available disk space (large-scale language models may need more).

Software requirements:

Node.js (v16 or later) and npm.

Git is used to clone the repository.

Ollama is used to run language models.

Terminals with bash or compatible shell.



### Deployment steps

1. Installation system dependency

Update the system and install the necessary tools.

```base
sudo apt update && sudo apt upgrade -y
```
```base
sudo apt install -y curl git build-essential
```

3. Install Node.js and npm

Use nvm to install Node.js and npm for flexible version management.

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



5. Install Ollama

Ollama is a necessary component for running the local language model, and it is installed using the official script.

```base
curl -fsSL https://ollama.com/install.sh | sh
```
Start the Ollama service and verify its running status.
```base
systemctl enable ollama
```
```base
systemctl start ollama
```
```base
ollama --version
```

Pull a language model (such as Llama 3.3) for Ollama to use.
```base
ollama pull llama3.3
```

4. Clone Gun-Ollama-Relay Warehouse

Clone the repository from GitHub.
```base
git clone https://github.com/ponzS/Gun-Ollama-Relay.git
```
```base
cd Gun-Ollama-Relay
```

6. Install project dependency

Install the JavaScript dependencies listed in package.json.
```base
npm install
```

8. Run the application

Start the Gun-Ollama-Relay application.

```base
node start.js
```
pm2 
```base
pm2 start start.js
```


7. Verify the deployment

Success terminal outputs the following content to represent successful operation
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

Local ollama API address: http://localhost:3939

LAN ollama API address: http://192.168.1.9:3939

GunDB database peer node relay url address: http://192.168.1.9:8765/gun

Whether the storage has been turned on: Storage: disabled

If you need to store it in the source code, change store = process.env.RELAY_STORE || false, change it to store = process.env.RELAY_STORE || true, or add a configuration file.


------Verify the running status in TalkFlow-----

Add the database peer node address to the relay page. Green means that the link has been established.

Create an identity for AI, add the ollama API address to the AI page, enable AI automatic reply, check your identity in the reply object, and try to send a message with your identity to establish communication with AI.

AI requires an independent client and identity operation to ensure that all messages are encrypted and facilitate the export of chat records for your training of private AI models and other purposes.

You can use TalkFlow in Lazy Cat directly as the identity of AI, and then communicate with it through the iOS version or desktop version. Or open more applications.

8. (Optional) Set as system service

To ensure the continuous operation of the application, create a systemd service.

Sudo nano /etc/systemd/system/gun-ollama-relay.service

Add the following content to adjust the path and user name:
[Unit]
Description=Gun-Ollama-Relay service
After=network.target

[Service]
ExecStart=/usr/bin/npm start --prefix /path/to/Gun-Ollama-Relay
WorkingDirectory=/path/to/Gun-Ollama-Relay
Restart=always
User=你的用户名
Environment=NODE_ENV=production

[Install]
WantedBy=multi-user.target

Enable and start the service.
sudo systemctl enable gun-ollama-relay
sudo systemctl start gun-ollama-relay
sudo systemctl status gun-ollama-relay

if you want to use pm2

npm install pm2

pm2 start start.js

9. How to open your ollama API and database co-to-equal nodes in the public network

(1). Download the LAN forwarding port in the lazy cat micro-service store

(2). Download the cloudflared tool in the lazy cat micro-service store

(3). Open ollama api and database ports, the default ollama is 3939, and the default gunDB port is 8765

For details, please refer to: https://github.com/wlabby-1/cloudflared-helper

Troubleshoot

Node.js error: Make sure to use the correct Node.js version (nvm use 20).

Ollama no response: Check whether the Ollama service is running (systemctl status ollama) and whether the model has been pulled.

Gun.js connection problem: Check the firewall settings to make sure that port 8765 (or the configured port) is open.

Log: View the application log (npm start output or systemd log, through journalctl -u gun-ollama-relay).

Notes

To monitor disk usage, the language model may take up a lot of space.

If you need further help, please check the problem page of the warehouse: https://github.com/ponzS/Gun-Ollama-Relay/issues

Thank you for your experience. We will maintain the TalkFlow version of Lazy Cat Microserver for a long time. Please submit suggestions for improvement at any time.
