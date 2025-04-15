# Gun-Vue@Relay -Ollama

Quick Start
```base
pnpm install
```

```base
node start.js
```

# Success
```base
=== GUN-VUE RELAY SERVER ===

Gun&Ollama Server running on http://localhost:3939
AXE relay enabled!
Internal URL: http://198.18.0.1:8765/
External URL: https://198.18.0.1/
Gun peer: http://198.18.0.1:8765/gun
Storage: disabled
Multicast on 233.255.255.255:8765
```

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













