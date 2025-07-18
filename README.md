# Gun-Vue@Relay -Ollama

<img src="https://raw.githubusercontent.com/DeFUCC/gun-vue/main/app/public/media/svg/relay.svg" alt="@gun-vue relay logo" width="400" />


# Skins 

https://github.com/DeFUCC/gun-vue/tree/main/relay/skin
 
 Quick Start

```base
sudo curl -fsSL --http1.1 https://ollama.com/install.sh | sh
```
 
 ```base
 pnpm install
 ```
 
 ```base
 node start.js
 ```

if pm2
```base
pm2 start start.js
```

 
 # Success
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
# GUN&Ollama Only 8765

from https://github.com/ponzS/Gun-Ollama
 
 # API
 
 Get Models -Get
 ```base
 http://localhost:3939/api/models
 ```
 
 Chat -text  -Post
 ```base
 http://localhost:3939/api/chat
 ```
 
 Generate completions  -Post
 ```base
 http://localhost:3939/api/generate
 ```
 
 Create Models  -Post
 ```base
 http://localhost:3939/api/models/create
 ```
 
 Delete Models  -Delete
 ```base
 http://localhost:3939/api/models/:name
 ```
 
 Copy Models  -Post
 ```base
 http://localhost:3939/api/models/copy
 ```
 
 Soul's Models  -Get
 ```base
 http://localhost:3939/api/models/:name
 ```
 
 Pull Models  -Post
 ```base
 http://localhost:3939/api/models/pull
 ```
 
 Push Models  -Post
 ```base
 http://localhost:3939/api/models/push
 ```
 
 Embeddings  -Post
 ```base
 http://localhost:3939/api/embeddings
 ```


learn more about gun-vue/relay : https://github.com/DeFUCC/gun-vue
