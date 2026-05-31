# WebSockets

## Definition

WebSockets provide **real-time communication** between client and server.

## Example

```javascript
// Client
const socket = new WebSocket('ws://localhost:3000');

socket.onopen = () => {
    socket.send('Hello Server');
};

socket.onmessage = (event) => {
    console.log(event.data);
};

// Server (Node.js)
const WebSocket = require('ws');
const wss = new WebSocket.Server({ port: 3000 });

wss.on('connection', (ws) => {
    ws.on('message', (data) => {
        ws.send(`Server received: ${data}`);
    });
});
```

## Quick Revision

- WebSockets = real-time bidirectional
- Persistent connection
- Use for: chat, live updates, gaming
- ws library for Node.js

---

## Related Topics

- [[What-is-WebSocket]] - [[What-is-WebSocket|WebSocket]]
- [[WebSockets]] - [[WebSockets|WebSockets]]
- [[What-is-API]] - [[What-is-API|APIs]]
- [[What-is-Node]] - [[What-is-Node|Node.js]]
