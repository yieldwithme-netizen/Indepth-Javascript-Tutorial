# What is WebSocket

## Definition

**WebSocket** is a communication protocol that provides full-duplex communication over a single TCP connection. Unlike HTTP, which is request-response based, WebSocket allows bidirectional, real-time data transfer between client and server.

## WebSocket vs HTTP

```javascript
// HTTP: Client initiates request, server responds
// Client: "Give me data"
// Server: "Here's the data"
// Connection closes or waits for next request

// WebSocket: Persistent connection
// Client: "Connect to me"
// Server: "Connected!"
// Both can send messages at any time
// Connection stays open
```

## Basic WebSocket Server

```javascript
const WebSocket = require('ws');
const http = require('http');

// Create HTTP server
const server = http.createServer();

// Create WebSocket server
const wss = new WebSocket.Server({ server });

// Store connected clients
const clients = new Set();

wss.on('connection', (ws) => {
  console.log('Client connected');
  clients.add(ws);
  
  // Send welcome message
  ws.send(JSON.stringify({
    type: 'welcome',
    message: 'Connected to WebSocket server'
  }));
  
  // Handle incoming messages
  ws.on('message', (message) => {
    try {
      const data = JSON.parse(message);
      console.log('Received:', data);
      
      // Broadcast to all clients
      clients.forEach((client) => {
        if (client.readyState === WebSocket.OPEN) {
          client.send(JSON.stringify(data));
        }
      });
    } catch (error) {
      console.error('Invalid message:', error);
    }
  });
  
  // Handle client disconnect
  ws.on('close', () => {
    console.log('Client disconnected');
    clients.delete(ws);
  });
  
  // Handle errors
  ws.on('error', (error) => {
    console.error('WebSocket error:', error);
    clients.delete(ws);
  });
});

// Start server
server.listen(8080, () => {
  console.log('WebSocket server running on port 8080');
});
```

## WebSocket Client

```javascript
// Browser WebSocket client
const ws = new WebSocket('ws://localhost:8080');

ws.onopen = () => {
  console.log('Connected to server');
  
  // Send message
  ws.send(JSON.stringify({
    type: 'chat',
    message: 'Hello Server!'
  }));
};

ws.onmessage = (event) => {
  const data = JSON.parse(event.data);
  console.log('Message from server:', data);
};

ws.onclose = () => {
  console.log('Disconnected from server');
};

ws.onerror = (error) => {
  console.error('WebSocket error:', error);
};

// Reconnection logic
function connectWithRetry() {
  const ws = new WebSocket('ws://localhost:8080');
  
  ws.onclose = () => {
    console.log('Connection closed, reconnecting in 3 seconds...');
    setTimeout(connectWithRetry, 3000);
  };
  
  return ws;
}
```

## Real-time Chat Application

```javascript
// Server
const WebSocket = require('ws');
const wss = new WebSocket.Server({ port: 8080 });

const messages = [];
const users = new Map();

wss.on('connection', (ws) => {
  ws.on('message', (data) => {
    const message = JSON.parse(data);
    
    switch (message.type) {
      case 'join':
        users.set(ws, message.username);
        broadcast({
          type: 'system',
          message: `${message.username} joined the chat`
        });
        break;
        
      case 'chat':
        const chatMessage = {
          type: 'chat',
          username: users.get(ws),
          message: message.text,
          timestamp: new Date().toISOString()
        };
        messages.push(chatMessage);
        broadcast(chatMessage);
        break;
        
      case 'typing':
        broadcast({
          type: 'typing',
          username: users.get(ws)
        }, ws);
        break;
    }
  });
  
  ws.on('close', () => {
    const username = users.get(ws);
    users.delete(ws);
    broadcast({
      type: 'system',
      message: `${username} left the chat`
    });
  });
});

function broadcast(data, exclude = null) {
  wss.clients.forEach((client) => {
    if (client.readyState === WebSocket.OPEN && client !== exclude) {
      client.send(JSON.stringify(data));
    }
  });
}
```

## Heartbeat/Ping-Pong

```javascript
// Keep connection alive
const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', (ws) => {
  ws.isAlive = true;
  
  ws.on('pong', () => {
    ws.isAlive = true;
  });
});

// Check connections every 30 seconds
const interval = setInterval(() => {
  wss.clients.forEach((ws) => {
    if (ws.isAlive === false) {
      return ws.terminate();
    }
    
    ws.isAlive = false;
    ws.ping();
  });
}, 30000);

wss.on('close', () => {
  clearInterval(interval);
});
```

## Common Mistakes

1. **Not handling reconnections**: Implement retry logic
2. **Missing heartbeat**: Prevent stale connections
3. **Not validating messages**: Always validate incoming data
4. **Ignoring connection state**: Check `readyState` before sending
5. **Memory leaks**: Clean up closed connections

## Related Topics

- [[What-is-REST]]
- [[What-is-CORS]]
- [[What-is-JWT]]

## Quick Revision

- WebSocket provides full-duplex, real-time communication
- Single persistent connection for bidirectional data
- Use `ws` library for Node.js servers
- Implement reconnection and heartbeat logic
- Validate all incoming messages
- Great for chat, live updates, and gaming
