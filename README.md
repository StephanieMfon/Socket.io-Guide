# Socket.io-Guide

## Prerequisites

Before getting started, make sure you have the following installed:

- React
- socket.io-client

## Installation

1. Install the socket.io-client package by running the following command in your React project:

   ```shell
   npm install socket.io-client
   ```

2. Import the socket.io-client library in your component file:

   ```javascript
   import io from 'socket.io-client';
   ```

## Socket.io Integration

### Connecting to the Socket.io Server

To establish a connection to the socket.io server, use the following code snippet:

```javascript
const socket = io();
```

Replace the default URL with your server's URL if it's different.

### Socket.io Events Listened For

The client listens for the following socket.io events:

#### `setup`

- Description: This event sets up the connection for a specific user by joining a room based on their `_id`.

```javascript
socket.on('setup', (userData) => {
  socket.join(userData._id);
  console.log(userData._id);
  socket.emit('connected');
});
```

#### `join chat`

- Description: This event allows the user to join a specific chat room.

```javascript
socket.on('join chat', (room) => {
  socket.join(room);
  console.log('User Joined Room: ' + room);
});
```

#### `new message`

- Description: This event receives a new message from the server and handles it accordingly.

```javascript
socket.on('new message', (newMessageReceived) => {
  // Handle new message logic
});
```

#### `disconnect`

- Description: This event is triggered when the client disconnects from the server.

```javascript
socket.on('disconnect', () => {
  console.log('User Disconnected');
});
```

### Socket.io Events Emitted

The client emits the following socket.io events:

#### `connected`

- Description: This event notifies the server that the client has successfully connected.

```javascript
socket.emit('connected');
```

#### `message received`

- Description: This event sends a received message to the corresponding user's room.

```javascript
socket.in(user._id).emit('message received', newMessageReceived);
```

## Example Usage

Here's an example usage to help you understand how to implement the socket.io events:

```javascript
import React, { useEffect } from 'react';
import io from 'socket.io-client';

const App = () => {
  useEffect(() => {
    // Connect to the socket.io server
    const socket = io();

    // Listen for 'setup' event
    socket.on('setup', (userData) => {
      socket.join(userData._id);
      console.log(userData._id);
      socket.emit('connected');
    });

    // Listen for 'join chat' event
    socket.on('join chat', (room) => {
      socket.join(room);
      console.log('User Joined Room: ' + room);
    });

    // Listen for 'new message' event
    socket.on('new message', (newMessageReceived) => {
      // Handle new message logic
    });

    // Listen for 'disconnect' event
    socket.on('disconnect', () => {
      console.log('User Disconnected');
    });

    // Emit 'connected' event
    socket.emit('connected');

    // Clean up the socket connection on component unmount
    return () => {
      socket.disconnect();
    };
 

 }, []);

  return <div>React App with Socket.io Integration</div>;
};

export default App;
```
