# Working with WebSocket in Node.js using TypeScript | by Vitaliy Korzhenko | Medium
[

![](https://miro.medium.com/v2/resize:fill:88:88/1*lzUS_0oJRHcI2N3rggc5Gg.jpeg)










](https://medium.com/@vitaliykorzenkoua?source=post_page-----1aebb8a06bd6--------------------------------)

![](https://miro.medium.com/v2/resize:fit:1400/1*7kGcXl7V8Enfra31aB5ELQ.jpeg)

WebSocket provides a powerful mechanism for establishing bidirectional communication channels between clients and servers. In Node.js, we can leverage the `ws` package along with TypeScript to conveniently implement WebSocket functionality. In this article, we will explore how to work with WebSocket in Node.js using TypeScript and provide examples to demonstrate their usage.

To get started, we need to install the `ws` package and TypeScript typings using npm:

```
$ npm install ws  
$ npm install --save-dev @types/ws
```

Let’s begin by creating a WebSocket server in Node.js using TypeScript. Save the following code in a file named `server.ts`

```
  
import WebSocket from 'ws';

const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', (ws: WebSocket) => {  
  console.log('New client connected');

  ws.on('message', (message: string) => {  
    console.log(`Received message: ${message}`);  
    ws.send(`Server received your message: ${message}`);  
  });

  ws.on('close', () => {  
    console.log('Client disconnected');  
  });  
});


```

In this code, we import the `WebSocket` type from the `ws` package and create a WebSocket server that listens on port 8080. When a client connects, a "connection" event is emitted, and we log a message to the console. When a message is received from the client, we log it and send a response back to the client.

Now let’s create a WebSocket client using TypeScript to connect to the server we just created. Save the following code in a file named `client.ts`:

```
import WebSocket from 'ws';

const ws = new WebSocket('ws://localhost:8080');

ws.on('open', () => {  
  console.log('Connected to server');

  ws.send('Hello, server!');  
});

ws.on('message', (message: string) => {  
  console.log(`Received message from server: ${message}`);  
});

ws.on('close', () => {  
  console.log('Disconnected from server');  
});


```

In this code, we import the `WebSocket` type from the `ws` package and create a WebSocket connection to the server at `ws://localhost:8080`. After the connection is established, an "open" event is emitted, and we log a message to the console. We then send a message to the server. When a message is received from the server, we log it.

WebSocket allows us to broadcast messages to multiple clients. Let’s modify our server code to broadcast messages to all connected clients. Update the `server.ts` file with the following code:

```
import WebSocket from 'ws';

const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', (ws: WebSocket) => {  
  console.log('New client connected');

  ws.on('message', (message: string) => {  
    console.log(`Received message: ${message}`);  
    wss.clients.forEach((client) => {  
      client.send(`Server received your message: ${message}`);  
    });  
  });

  ws.on('close', () => {  
    console.log('Client disconnected');  
  });  
});


```

In this modified code, when a message is received from a client, we iterate over all connected clients using the `wss.clients` property and send the message to each one.

WebSocket in Node.js using TypeScript provides a powerful mechanism for real-time communication between clients and servers. In this article, we explored how to work with WebSocket in Node.js using the `ws` package with TypeScript typings. We covered the basics of creating a WebSocket server, connecting a client to the server, and broadcasting messages to all connected clients. Feel free to explore further and leverage the full potential of WebSocket in your Node.js applications.