# Environment Setup & Tools

---

## 🎯 What You'll Build

By the end of this module, you'll have a complete gRPC development environment with:
- ✅ Node.js configured for gRPC development
- ✅ All necessary packages installed and tested
- ✅ Professional project structure
- ✅ Development tools and scripts ready
- ✅ Your first working "Hello World" gRPC service

---

## 📋 Prerequisites Check

Before we start, let's verify your system is ready:

### Required Software
- **Node.js**: Version 14.x or higher (18.x+ recommended)
- **npm**: Version 6.x or higher (comes with Node.js)
- **Git**: For version control
- **Code Editor**: VS Code recommended (with extensions)

### Quick System Check
```bash
# Check Node.js version
node --version    # Should show v14+ (v18+ preferred)

# Check npm version  
npm --version     # Should show v6+

# Check Git
git --version     # Any recent version is fine

# Check if you have a C++ compiler (needed for some gRPC packages)
# Windows: Install Visual Studio Build Tools
# macOS: Install Xcode Command Line Tools
# Linux: Install build-essential
```

---

## 🔧 Development Environment Setup

### Step 1: Node.js Version Management

If you don't have the right Node.js version, I recommend using a version manager:

{% tabs %}

{% tab title="Using nvm (macOS/Linux)" %} 
Install nvm if you haven't already.
- Open up the terminal and run the following
  ```bash
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
  ```
- Restart terminal or source your profile
  ```bash
  source ~/.bashrc  # or ~/.zshrc
  ```
- Install and use Node.js 18(or higher)
  ```bash
  nvm install 18
  nvm use 18
  nvm alias default 18
  ```
- Verify
  ```bash
  node --version  # Should show v18.x.x
  ```
{% endtab %}

{% tab title="Using nvm-windows (Windows)" %}
```bash
# Download from: https://github.com/coreybutler/nvm-windows/releases
# Install and then:
nvm install 18.17.0
nvm use 18.17.0
```

{% endtab %}

{% endtabs %}

### Step 2: Create Your Project Directory

```bash
# Create main project directory
mkdir grpc-nodejs-mastery
cd grpc-nodejs-mastery

# Initialize git repository
git init

# Create basic structure
mkdir -p {lessons,exercises,projects,notes,tools}

# Create .gitignore
cat > .gitignore << EOF
# Dependencies
node_modules/
npm-debug.log*

# Environment files
.env
.env.local

# IDE files
.vscode/settings.json
.idea/

# OS files
.DS_Store
Thumbs.db

# Build outputs
dist/
build/
*.tgz

# Logs
logs/
*.log
EOF
```

---

## 📦 Installing gRPC Packages

### Core gRPC Packages

Let's create a test project to verify our setup:

```bash
# Navigate to exercises directory
cd exercises
mkdir module-2-setup
cd module-2-setup

# Initialize npm project
npm init -y

# Install core gRPC packages
npm install @grpc/grpc-js @grpc/proto-loader

# Install development dependencies
npm install --save-dev nodemon

# Verify installation
npm list @grpc/grpc-js @grpc/proto-loader
```

### Package Breakdown

| Package | Purpose | Size |
|---------|---------|------|
| `@grpc/grpc-js` | Pure JavaScript gRPC implementation | ~2MB |
| `@grpc/proto-loader` | Loads .proto files at runtime | ~500KB |

**Why these packages?**
- **@grpc/grpc-js**: Pure JS implementation (no native dependencies!)
- **@grpc/proto-loader**: Dynamic proto loading (easier development)

### Alternative: Static Code Generation (Optional)

For production applications, you might prefer static code generation:

```bash
# Install protobuf compiler and tools
npm install --save-dev grpc-tools

# This gives you:
# - protoc: Protocol Buffers compiler
# - grpc_tools_node_protoc_plugin: gRPC Node.js plugin
```

---

## 🛠️ Essential Development Tools

### 1. grpcurl - The gRPC Swiss Army Knife
{% tabs %}



{% tab title="OSX" %} 
```bash
brew install grpcurl
```
{% endtab %}

{% tab title="Linux" %}
```bash
# Download latest release from GitHub
curl -L https://github.com/fullstorydev/grpcurl/releases/latest/download/grpcurl_1.8.7_linux_x86_64.tar.gz | tar -xz
sudo mv grpcurl /usr/local/bin/
```
{% endtab %}

{% tab title="Windows" %}
```bash
# Using Chocolatey
choco install grpcurl

# Or download from GitHub releases
```
{% endtab %}

{% endtabs %}

**Test grpcurl:**
```bash
grpcurl --version
```

### 2. VS Code Extensions

Install these extensions for the best development experience:

```bash
# Open VS Code in your project
code .

# Install extensions (manually or via command palette)
```

**Recommended Extensions:**
- **vscode-proto3**: Protocol Buffers syntax highlighting
- **REST Client**: Test HTTP/gRPC endpoints
- **GitLens**: Enhanced Git capabilities
- **Prettier**: Code formatting
- **ESLint**: JavaScript linting

### 3. Postman with gRPC Support

Modern versions of Postman support gRPC natively:
1. Download Postman from https://www.postman.com/
2. Enable gRPC support in settings
3. We'll use this for testing our services

---

## 📁 Project Structure

Let's create a scalable project structure:

```bash
# In your exercises/module-2-setup directory
mkdir -p {src/{proto,services,client,utils},tests,scripts,docs}

# Create the structure
tree . || ls -la  # View structure
```

### Recommended Structure

```
grpc-nodejs-mastery/
├── lessons/                    # Markdown lesson files
├── exercises/
│   └── module-2-setup/
│       ├── src/
│       │   ├── proto/          # .proto files
│       │   ├── services/       # Service implementations
│       │   ├── client/         # Client code
│       │   └── utils/          # Helper utilities
│       ├── tests/              # Test files
│       ├── scripts/            # Build/dev scripts
│       ├── docs/               # Documentation
│       ├── package.json
│       └── README.md
├── projects/                   # Major projects
└── notes/                      # Personal notes
```

### Create Essential Files

{% code title="README.md" overflow="wrap" lineNumbers="true" %}

```markdown
‌# Module 2: gRPC Environment Setup

This project verifies our gRPC development environment setup.

## Quick Start
npm install
npm run dev
```

{% endcode %}


## Scripts

- `npm run dev`: Start development server
- `npm test`: Run tests
- `npm run proto`: Generate code from proto files


# Create package.json scripts
```bash
npm pkg set scripts.dev="nodemon src/server.js"
npm pkg set scripts.start="node src/server.js"
npm pkg set scripts.test="echo 'Tests not implemented yet'"
npm pkg set scripts.proto="echo 'Proto compilation not configured yet'"
```


---

## 🎯 First gRPC Service: Hello World

Let's build your first gRPC service to verify everything works!

### Step 1: Define the Service (hello.proto)
{% code title="hello.proto" overflow="wrap" lineNumbers="true" %}

```protobuf
‌syntax = "proto3";

package hello;

// The greeting service definition
service HelloService {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloResponse) {}
  
  // Server streaming: sends multiple greetings
  rpc SayHelloStream (HelloRequest) returns (stream HelloResponse) {}
}

// The request message containing the user's name
message HelloRequest {
  string name = 1;
  string language = 2;
}

// The response message containing the greeting
message HelloResponse {
  string message = 1;
  int64 timestamp = 2;
}
```


### Step 2: Implement the Server

{% code title="src/server.js" overflow="wrap" lineNumbers="true" %}

```javascript
const grpc = require('@grpc/grpc-js');
const protoLoader = require('@grpc/proto-loader');
const path = require('path');

// Load the protobuf definition
const PROTO_PATH = path.join(__dirname, 'proto', 'hello.proto');
const packageDefinition = protoLoader.loadSync(PROTO_PATH, {
  keepCase: true,
  longs: String,
  enums: String,
  defaults: true,
  oneofs: true
});

const helloProto = grpc.loadPackageDefinition(packageDefinition).hello;

// Implement the service methods
const helloService = {
  // Unary RPC: SayHello
  SayHello: (call, callback) => {
    const { name, language } = call.request;
    
    // Simple greeting logic based on language
    const greetings = {
      'en': `Hello, ${name}!`,
      'es': `¡Hola, ${name}!`,
      'fr': `Bonjour, ${name}!`,
      'de': `Hallo, ${name}!`,
      'default': `Hello, ${name}!`
    };
    
    const message = greetings[language] || greetings.default;
    
    console.log(`Received request: name=${name}, language=${language}`);
    
    callback(null, {
      message: message,
      timestamp: Date.now().toString()
    });
  },

  // Server Streaming RPC: SayHelloStream
  SayHelloStream: (call) => {
    const { name, language } = call.request;
    
    console.log(`Streaming request: name=${name}, language=${language}`);
    
    // Send 5 greetings with 1 second intervals
    let count = 0;
    const interval = setInterval(() => {
      count++;
      
      call.write({
        message: `Hello ${name} - Message ${count}`,
        timestamp: Date.now().toString()
      });
      
      if (count >= 5) {
        clearInterval(interval);
        call.end();
      }
    }, 1000);
    
    // Handle client disconnect
    call.on('cancelled', () => {
      console.log('Client cancelled the stream');
      clearInterval(interval);
    });
  }
};

// Create and start the server
function startServer() {
  const server = new grpc.Server();
  
  // Add the service
  server.addService(helloProto.HelloService.service, helloService);
  
  // Bind the server to a port
  const serverAddress = '0.0.0.0:50051';
  server.bindAsync(
    serverAddress,
    grpc.ServerCredentials.createInsecure(),
    (error, port) => {
      if (error) {
        console.error('Failed to start server:', error);
        return;
      }
      
      console.log(`✅ gRPC Server started successfully!`);
      console.log(`🚀 Server running at http://localhost:${port}`);
      console.log(`📡 Ready to accept gRPC connections...`);
      
      server.start();
    }
  );
}

// Handle graceful shutdown
process.on('SIGINT', () => {
  console.log('\n🛑 Received SIGINT, shutting down gracefully...');
  process.exit(0);
});

// Start the server
startServer();
```

{% endcode %}


### Step 3: Create a Test Client

{% code title="src/client.js" overflow="wrap" lineNumbers="true" %}

```javascript
‌const grpc = require('@grpc/grpc-js');
const protoLoader = require('@grpc/proto-loader');
const path = require('path');

// Load the protobuf definition
const PROTO_PATH = path.join(__dirname, 'proto', 'hello.proto');
const packageDefinition = protoLoader.loadSync(PROTO_PATH, {
  keepCase: true,
  longs: String,
  enums: String,
  defaults: true,
  oneofs: true
});

const helloProto = grpc.loadPackageDefinition(packageDefinition).hello;

// Create client
const client = new helloProto.HelloService(
  'localhost:50051',
  grpc.credentials.createInsecure()
);

// Test functions
function testUnaryCall() {
  console.log('\n🔥 Testing Unary RPC: SayHello');
  
  const request = {
    name: 'World',
    language: 'en'
  };
  
  client.SayHello(request, (error, response) => {
    if (error) {
      console.error('❌ Error:', error);
      return;
    }
    
    console.log('✅ Response:', response);
  });
}

function testStreamingCall() {
  console.log('\n🌊 Testing Server Streaming RPC: SayHelloStream');
  
  const request = {
    name: 'Streaming World',
    language: 'en'
  };
  
  const stream = client.SayHelloStream(request);
  
  stream.on('data', (response) => {
    console.log('📨 Received:', response);
  });
  
  stream.on('error', (error) => {
    console.error('❌ Stream error:', error);
  });
  
  stream.on('end', () => {
    console.log('✅ Stream ended');
  });
}

// Test different languages
function testMultipleLanguages() {
  console.log('\n🌍 Testing Multiple Languages');
  
  const languages = ['en', 'es', 'fr', 'de'];
  
  languages.forEach((language, index) => {
    setTimeout(() => {
      client.SayHello(
        { name: 'Polyglot', language },
        (error, response) => {
          if (error) {
            console.error(`❌ Error for ${language}:`, error);
          } else {
            console.log(`✅ ${language.toUpperCase()}:`, response.message);
          }
        }
      );
    }, index * 500);
  });
}

// Run tests
console.log('🚀 Starting gRPC Client Tests...');

// Wait a bit for server to be ready
setTimeout(() => {
  testUnaryCall();
  
  setTimeout(() => {
    testStreamingCall();
  }, 2000);
  
  setTimeout(() => {
    testMultipleLanguages();
  }, 8000);
}, 1000);

```

{% endcode %}


### Step 4: Create Utility Scripts

{% code title="src/utils/health-check.js" overflow="wrap" lineNumbers="true" %}

```javascript
‌const grpc = require('@grpc/grpc-js');
const protoLoader = require('@grpc/proto-loader');
const path = require('path');

function checkServerHealth() {
  console.log('🏥 Checking gRPC server health...');
  
  // Load proto
  const PROTO_PATH = path.join(__dirname, '..', 'proto', 'hello.proto');
  const packageDefinition = protoLoader.loadSync(PROTO_PATH, {
    keepCase: true,
    longs: String,
    enums: String,
    defaults: true,
    oneofs: true
  });
  
  const helloProto = grpc.loadPackageDefinition(packageDefinition).hello;
  
  // Create client
  const client = new helloProto.HelloService(
    'localhost:50051',
    grpc.credentials.createInsecure()
  );
  
  // Test connection
  const deadline = new Date();
  deadline.setSeconds(deadline.getSeconds() + 5);
  
  client.waitForReady(deadline, (error) => {
    if (error) {
      console.log('❌ Server is not ready:', error.message);
      process.exit(1);
    } else {
      console.log('✅ Server is healthy and ready!');
      
      // Make a test call
      client.SayHello(
        { name: 'Health Check', language: 'en' },
        (error, response) => {
          if (error) {
            console.log('❌ Test call failed:', error.message);
            process.exit(1);
          } else {
            console.log('✅ Test call successful:', response.message);
            process.exit(0);
          }
        }
      );
    }
  });
}

checkServerHealth();

```

{% endcode %}

# Add health check script to package.json
npm pkg set scripts.health="node src/utils/health-check.js"

---

## 🧪 Testing Your Setup

Now let's test everything works!

### Terminal 1: Start the Server

```bash
# In your module-2-setup directory
npm run dev

# You should see:
# ✅ gRPC Server started successfully!
# 🚀 Server running at http://localhost:50051
# 📡 Ready to accept gRPC connections...
```

### Terminal 2: Run the Client

```bash
# In another terminal, same directory
node src/client.js

# You should see various test outputs including:
# 🔥 Testing Unary RPC: SayHello
# ✅ Response: { message: 'Hello, World!', timestamp: '...' }
# 🌊 Testing Server Streaming RPC: SayHelloStream
# ... streaming responses ...
```

### Terminal 3: Health Check

```bash
# Test the health check utility
npm run health

# Should output:
# ✅ Server is healthy and ready!
# ✅ Test call successful: Hello, Health Check!
```

---

## 🔍 Testing with grpcurl

Let's test with external tools:

```bash
# List available services
grpcurl -plaintext localhost:50051 list

# Describe the HelloService
grpcurl -plaintext localhost:50051 describe hello.HelloService

# Make a unary call
grpcurl -plaintext -d '{"name": "grpcurl", "language": "en"}' \
  localhost:50051 hello.HelloService/SayHello

# Test server streaming (press Ctrl+C to stop)
grpcurl -plaintext -d '{"name": "grpcurl streaming", "language": "es"}' \
  localhost:50051 hello.HelloService/SayHelloStream
```

---

## 🐛 Troubleshooting Common Issues

### Issue 1: "Cannot find module '@grpc/grpc-js'"
```bash
# Solution: Reinstall packages
rm -rf node_modules package-lock.json
npm install
```

### Issue 2: "Server failed to bind"
```bash
# Solution: Port already in use
lsof -i :50051  # Find what's using the port
kill -9 <PID>   # Kill the process
```

### Issue 3: "No such file or directory: proto file"
```bash
# Solution: Check proto file path
ls -la src/proto/hello.proto  # Verify file exists
```

### Issue 4: Node.js version issues
```bash
# Solution: Use correct Node.js version
nvm use 18
node --version  # Verify
```

---

## 📊 Environment Verification Checklist

Mark each item as you verify it works:

- [x] **Node.js 14+** installed and active
- [x] **gRPC packages** installed successfully
- [x] **Project structure** created properly
- [x] **Hello World server** starts without errors
- [x] **Client** can connect and make calls
- [x] **Streaming RPC** works correctly
- [x] **grpcurl** can connect and make calls
- [x] **Health check** passes
- [x] **Development scripts** work (npm run dev, etc.)
- [x] **VS Code extensions** installed and working

---

## 🎯 Your Challenge: Customize the Service

Before moving to the next module, try these challenges:

### Challenge 1: Add a New RPC Method
Add a `SayGoodbye` method to the service:
```protobuf
rpc SayGoodbye (GoodbyeRequest) returns (GoodbyeResponse) {}
```

### Challenge 2: Add Error Handling
Implement proper error handling for invalid languages or empty names.

### Challenge 3: Add Metadata
Learn to send and receive metadata (headers) in your gRPC calls.

<details>
<summary>🔍 Click for hints</summary>

**Challenge 1 Hint:**
```protobuf
message GoodbyeRequest {
  string name = 1;
}

message GoodbyeResponse {
  string farewell = 1;
  int64 timestamp = 2;
}
```

**Challenge 2 Hint:**
```javascript
// In service implementation
if (!name || name.trim() === '') {
  callback({
    code: grpc.status.INVALID_ARGUMENT,
    details: 'Name is required'
  });
  return;
}
```

**Challenge 3 Hint:**
```javascript
// Server: reading metadata
const metadata = call.metadata;
console.log('Request metadata:', metadata.getMap());

// Client: sending metadata
const metadata = new grpc.Metadata();
metadata.add('client-type', 'test');
client.SayHello(request, metadata, callback);
```

</details>

---

## 📚 Key Takeaways

- **@grpc/grpc-js**: Pure JavaScript gRPC implementation (no native deps!)
- **@grpc/proto-loader**: Dynamic protobuf loading for development
- **Project structure**: Organize proto, services, and client code clearly
- **Development workflow**: Server + client + testing tools
- **grpcurl**: Essential tool for testing and debugging gRPC services

---


In Module 3, we'll dive deeper into:
- Building more complex gRPC services
- Advanced error handling patterns
- Proper testing strategies
- Service composition and organization

---

## 📖 Additional Resources

- [gRPC Node.js Documentation](https://grpc.github.io/grpc/node/)
- [@grpc/proto-loader API](https://www.npmjs.com/package/@grpc/proto-loader)
- [grpcurl Documentation](https://github.com/fullstorydev/grpcurl)
- [VS Code Proto3 Extension](https://marketplace.visualstudio.com/items?itemName=zxh404.vscode-proto3)

---

**💡 Pro Tip:** Save your working setup! This module-2-setup project makes a great template for future gRPC projects. You can copy this structure whenever you start a new gRPC service.
