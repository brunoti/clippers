# Node.js — Usando WebSockets. WebSockets são uma ferramenta para… | by Eduardo Rabelo | Medium
[

![](https://miro.medium.com/v2/resize:fill:88:88/1*yLyHDumpDQY3fJ1Y0BL28A.jpeg)










](https://oieduardorabelo.medium.com/?source=post_page-----5d642456d1f3--------------------------------)

![](https://miro.medium.com/v2/resize:fit:1400/1*-hJFTVNzMWdi0A5lH5s8jg.png)

[WebSockets](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket) são uma ferramenta para comunicação bidirecional entre um cliente de navegador e um servidor. Em particular, os WebSockets permitem que o servidor envie dados para o cliente. Isso é diferente de sua solicitação HTTP padrão usando `[fetch()](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)` ou [Axios](https://masteringjs.io/tutorials/axios/basic_auth) porque o servidor não pode se comunicar com o cliente a menos que o cliente envie uma solicitação primeiro.

WebSockets são mais flexíveis, mas também são mais difíceis de implementar e escalar. Os WebSockets sobrecarregam ainda mais o desenvolvedor, portanto, use-os com moderação e apenas quando for absolutamente necessário. Neste artigo, você aprenderá como construir um aplicativo simples de bate-papo em tempo real usando WebSockets.

O [pacote](https://www.npmjs.com/package/ws) `[ws](https://www.npmjs.com/package/ws)` [npm](https://www.npmjs.com/package/ws) é a "de fato" biblioteca WebSocket para Node.js. Você também pode usar [Socket.IO](https://socket.io/), mas Socket.IO é uma abstração de nível superior sobre WebSockets, ao invés de uma implementação do [protocolo WebSocket](https://tools.ietf.org/html/rfc6455).

Abaixo está um exemplo básico de um servidor WebSocket que rastreia todos os sockets abertos e envia mensagens de entrada para todos os sockets abertos. Você pode pensar nisso como um simples servidor de bate-papo: quando uma pessoa envia uma mensagem, o servidor a transmite para todos que estão ouvindo.

```
const WebSocket = require('ws');  
const server = new WebSocket.Server({  
  port: 8080  
});let sockets = \[\];  
server.on('connection', function(socket) {  
  // Adicionamos cada nova conexão/socket ao array \`sockets\`  
  sockets.push(socket); // Quando você receber uma mensagem, enviamos ela para todos os sockets  
  socket.on('message', function(msg) {  
    sockets.forEach(s => s.send(msg));  
  }); // Quando a conexão de um socket é fechada/disconectada, removemos o socket do array  
  socket.on('close', function() {  
    sockets = sockets.filter(s => s !== socket);  
  });  
});
```

Uma conexão WebSocket possui dois componentes, um cliente e um servidor. No exemplo acima, você criou um servidor. Os clientes iniciam uma solicitação para abrir uma conexão WebSocket e os servidores respondem às solicitações de entrada para abrir conexões WebSocket.

Você também pode criar um cliente WebSocket em Node.js usando `ws`. Isso é ótimo para testar sua lógica WebSocket, embora você também possa usar WebSockets para comunicação entre serviços de back-end. Abaixo está um exemplo de um cliente WebSocket que se comunica com o servidor acima.

```
let clients = \[  
  new WebSocket('ws://localhost:8080'),  
  new WebSocket('ws://localhost:8080')  
\];clients.map(client => {  
  client.on('message', msg => console.log(msg));  
});// Esperamos o cliente conectar com o servidor usando async/await  
await new Promise(resolve => clients\[0\].once('open', resolve));// Imprimi "Hello!" duas vezes, um para cada cliente  
clients\[0\].send('Hello!');
```

[A maioria dos navegadores modernos oferece suporte para WebSockets por padrão](https://caniuse.com/#feat=websockets) . Em outras palavras, você pode usar a classe `WebSocket` no navegador sem `ws` ou sem transpiladores, a menos que queira oferecer suporte ao Internet Explorer 9 ou Opera Mini. Abaixo está uma imagem [da seção de WebSockets do](https://caniuse.com/#feat=websockets) `[caniuse.com](https://caniuse.com/#feat=websockets)`.

![](https://miro.medium.com/v2/resize:fit:1400/0*9sIo3tMkOk-P5V0M.png)

E aqui está um exemplo de uma página de chat que se conecta ao servidor do início desse artigo:

```
<html>  
  <head>  
    <script type="text/javascript">  
      const ws = new WebSocket('ws://localhost:8080'); // A classe \`WebSocket\` nos navegadores tem uma sintaxe um pouco diferente de \`ws\`  
      // Ao invés da sintax de EventEmmiter \`on('open')\`, você adiciona um callback  
      // a propriedade \`onopen\`.  
      ws.onopen = function() {  
        document.querySelector('#send').disabled = false; document.querySelector('#send').addEventListener('click', function() {  
          ws.send(document.querySelector('#message').value);  
        });  
      }; ws.onmessage = function(msg) {  
        document.querySelector('#messages').innerHTML += `<div>${msg.data}</div>`;  
      };  
    </script>  
  </head>  
  <body>  
    <h1>Chat</h1>  
    <div>  
      <input id="message" placeholder="Message">  
      <button id="send" disabled="true">Send</button>  
    </div>  
    <div id="messages">  
    </div>  
  </body>  
</html>
```

Observe que os WebSockets no navegador têm uma sintaxe ligeiramente diferente para [aguardar a conexão](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_client_applications#Sending_data_to_the_server) e [receber mensagens do servidor](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_client_applications#Receiving_messages_from_the_server) . Ao invés de `on('message', messageHandler)`, você deve escrever `onmessage = messageHandler`.

*   [WebSockets in Node.js](https://masteringjs.io/tutorials/node/websockets), escrito originalmente por [Valeri Karpov](https://twitter.com/code_barbarian).