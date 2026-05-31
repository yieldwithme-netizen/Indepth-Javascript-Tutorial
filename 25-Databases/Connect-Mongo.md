# How to Connect to MongoDB

## Definition

Connecting to MongoDB in a Node.js application involves using the Mongoose library to establish a connection to the MongoDB server. This connection allows your application to perform CRUD operations on the database.

```javascript
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost:27017/myapp');
```

## Basic Connection

```javascript
const mongoose = require('mongoose');

// Connect to MongoDB
async function connectDB() {
  try {
    await mongoose.connect('mongodb://localhost:27017/myapp');
    console.log('Connected to MongoDB successfully');
  } catch (error) {
    console.error('Error connecting to MongoDB:', error.message);
    process.exit(1);
  }
}

connectDB();
```

## Connection Options

```javascript
const mongoose = require('mongoose');

const options = {
  // Server selection timeout
  serverSelectionTimeoutMS: 5000,
  
  // Socket timeout
  socketTimeoutMS: 45000,
  
  // Connection timeout
  connectTimeoutMS: 10000,
  
  // Heartbeat frequency
  heartbeatFrequencyMS: 10000,
  
  // Auto reconnect
  autoReconnect: true,
  
  // Max pool size
  maxPoolSize: 10,
  
  // Min pool size
  minPoolSize: 1,
  
  // Use unified topology
  useUnifiedTopology: true,
  
  // Use new URL parser
  useNewUrlParser: true
};

async function connectDB() {
  try {
    await mongoose.connect('mongodb://localhost:27017/myapp', options);
    console.log('Connected to MongoDB successfully');
  } catch (error) {
    console.error('Error connecting to MongoDB:', error.message);
    process.exit(1);
  }
}

connectDB();
```

## Connection String Formats

### Local Connection

```javascript
// Simple local connection
await mongoose.connect('mongodb://localhost:27017/myapp');

// With authentication
await mongoose.connect('mongodb://username:password@localhost:27017/myapp');

// With authentication database
await mongoose.connect('mongodb://username:password@localhost:27017/myapp?authSource=admin');
```

### MongoDB Atlas (Cloud)

```javascript
// Atlas connection string
await mongoose.connect('mongodb+srv://username:password@cluster0.mongodb.net/myapp?retryWrites=true&w=majority');

// With options
const options = {
  useNewUrlParser: true,
  useUnifiedTopology: true,
  retryWrites: true,
  w: 'majority'
};

await mongoose.connect('mongodb+srv://username:password@cluster0.mongodb.net/myapp', options);
```

### Replica Set

```javascript
// Replica set connection
await mongoose.connect('mongodb://server1:27017,server2:27017,server3:27017/myapp?replicaSet=myReplicaSet');
```

### Shard Cluster

```javascript
// Shard cluster connection
await mongoose.connect('mongodb://mongos1:27017,mongos2:27017/myapp');
```

## Environment Variables

```javascript
// .env file
MONGODB_URI=mongodb://localhost:27017/myapp
MONGODB_URI_PRODUCTION=mongodb+srv://user:pass@cluster.mongodb.net/myapp

// config.js
require('dotenv').config();

module.exports = {
  mongodb: {
    uri: process.env.MONGODB_URI || 'mongodb://localhost:27017/myapp',
    options: {
      useNewUrlParser: true,
      useUnifiedTopology: true
    }
  }
};

// app.js
const mongoose = require('mongoose');
const config = require('./config');

async function connectDB() {
  try {
    await mongoose.connect(config.mongodb.uri, config.mongodb.options);
    console.log('Connected to MongoDB');
  } catch (error) {
    console.error('MongoDB connection error:', error);
    process.exit(1);
  }
}
```

## Connection Events

```javascript
const mongoose = require('mongoose');

// Connection events
mongoose.connection.on('connected', () => {
  console.log('Mongoose connected to MongoDB');
});

mongoose.connection.on('error', (err) => {
  console.error('Mongoose connection error:', err);
});

mongoose.connection.on('disconnected', () => {
  console.log('Mongoose disconnected from MongoDB');
});

// Close connection on process termination
process.on('SIGINT', async () => {
  await mongoose.connection.close();
  console.log('MongoDB connection closed due to app termination');
  process.exit(0);
});

// Connect to database
async function connectDB() {
  try {
    await mongoose.connect('mongodb://localhost:27017/myapp');
  } catch (error) {
    console.error('Error connecting to MongoDB:', error);
    process.exit(1);
  }
}

connectDB();
```

## Connection Utility File

```javascript
// utils/db.js
const mongoose = require('mongoose');

let isConnected = false;

const connectDB = async () => {
  if (isConnected) {
    console.log('Using existing database connection');
    return;
  }

  try {
    const conn = await mongoose.connect(process.env.MONGODB_URI, {
      useNewUrlParser: true,
      useUnifiedTopology: true,
      useCreateIndex: true,
      useFindAndModify: false
    });

    isConnected = true;
    console.log(`MongoDB Connected: ${conn.connection.host}`);
  } catch (error) {
    console.error(`Error: ${error.message}`);
    process.exit(1);
  }
};

mongoose.connection.on('disconnected', () => {
  isConnected = false;
  console.log('MongoDB disconnected');
});

mongoose.connection.on('error', (err) => {
  console.error(`MongoDB connection error: ${err}`);
});

module.exports = connectDB;
```

## Complete Application Setup

```javascript
// app.js
const express = require('express');
const mongoose = require('mongoose');
require('dotenv').config();

const app = express();

// Middleware
app.use(express.json());

// Database connection
const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGODB_URI, {
      useNewUrlParser: true,
      useUnifiedTopology: true
    });
    console.log('MongoDB connected successfully');
  } catch (error) {
    console.error('Database connection failed:', error.message);
    process.exit(1);
  }
};

// Connect to database
connectDB();

// Routes
app.get('/', (req, res) => {
  res.json({ message: 'API is running' });
});

// Error handling middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Something went wrong!' });
});

// Start server
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

## Common Use Cases

- **Web Applications**: Connect to user database
- **APIs**: Backend database for REST/GraphQL APIs
- **Microservices**: Service-specific database connections
- **Real-time Apps**: Chat and notification systems
- **E-commerce**: Product and order databases
- **Content Management**: Article and page storage

## Common Mistakes

1. **Not handling connection errors**: Always catch connection errors
2. **Hardcoding connection strings**: Use environment variables
3. **Not closing connections**: Handle process termination properly
4. **Multiple connections**: Reuse connections instead of creating new ones
5. **Missing options**: Use recommended connection options

```javascript
// WRONG - Not handling errors
mongoose.connect('mongodb://localhost:27017/myapp');

// CORRECT - Handle errors
try {
  await mongoose.connect('mongodb://localhost:27017/myapp');
  console.log('Connected');
} catch (error) {
  console.error('Connection failed:', error);
}

// WRONG - Hardcoded connection string
await mongoose.connect('mongodb://user:pass@localhost:27017/myapp');

// CORRECT - Use environment variables
await mongoose.connect(process.env.MONGODB_URI);

// WRONG - Creating multiple connections
async function getUser() {
  await mongoose.connect(process.env.MONGODB_URI); // New connection each time!
  const user = await User.findOne();
  return user;
}

// CORRECT - Single connection
let isConnected = false;

async function connectDB() {
  if (!isConnected) {
    await mongoose.connect(process.env.MONGODB_URI);
    isConnected = true;
  }
}
```

## Quick Revision

- Use `mongoose.connect()` to establish connection
- Connection string format: `mongodb://host:port/database`
- MongoDB Atlas: `mongodb+srv://user:pass@cluster.mongodb.net/db`
- Always handle connection errors with try-catch
- Use environment variables for connection strings
- Handle connection events (connected, error, disconnected)
- Close connection on process termination
- Use connection pooling for better performance
- Recommended options: `useNewUrlParser`, `useUnifiedTopology`

## Related Topics

- [[What-is-MongoDB]]
- [[What-is-Mongoose]]
- [[Perform-CRUD]]
- [[Environment-Variables]]
- [[Error-Handling]]
