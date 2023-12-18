# Como criar um servidor de WebSockets em Node.js – LuizTools
Em aplicações cliente-servidor tradicionais, o cliente envia requests para o servidor, que por sua vez responde de maneira síncrona. Mas e quando queremos que o servidor envie mensagens para o cliente, como fazer? Por exemplo, após o processamento de uma tarefa demorada, complexa, que não conseguiríamos responder diretamente a um request…Algumas empresas tem a brilhante ideia de fazer polling, que nada mais é do que ficar fazendo requests de tempos em tempos para ver se o servidor tem alguma novidade, uma técnica que até funciona, mas que ao mesmo tempo é extremamente ineficiente.

WebSocket é uma tecnologia que permite comunicação bi-lateral usando uma única conexão (socket) entre o cliente e o servidor. Assim, tanto o cliente pode enviar requests, quando o server, a hora que quiserem após a conexão ser estabelecida. Suportado em todos os browsers modernos, permite criarmos qualquer tipo de troca de informação assíncrona e leve, o que é ideal para aplicações real-time e front-ends reativos ([as famosas SPAs ou Single Page Applications](https://www.luiztools.com.br/post/tutorial-de-react-js-com-node-js/)).

Em um tutorial aqui do blog, ensino a usar a biblioteca [Socket.io](https://www.luiztools.com.br/post/criando-um-chat-com-nodejs-e-socketio/) para construção de uma aplicação de chat baseada em websockets, inclusive com um [cliente para Android](https://www.luiztools.com.br/post/tutorial-app-de-mensagens-para-android-com-backend-em-nodejs/). No tutorial de hoje, eu vou ensinar como você pode construir um servidor de websockets em Node.js usando a biblioteca WS, a mais popular e leve, para servir dados assíncronos em real-time para seus clientes.

Veremos neste tutorial:

1.  [Setup do Projeto](#1)
2.  [Setup do servidor de WebSockets](#2)
3.  [Testando o Websocket Server](#3)

Vamos lá!

_**Atenção:** este tutorial é para criação de um servidor de WebSockets. Caso esteja procurando como criar um cliente de websockets, use [este outro tutorial](https://www.luiztools.com.br/post/como-criar-um-cliente-de-websockets-em-node-js/)._

[![](https://www.luiztools.com.br/wp-content/uploads/2021/06/banner-ebooks-node.jpg)
](https://www.luiztools.com.br/mongodb-ou-mysql/)  

### #1 – Setup do Projeto

Crie uma nova pasta chamada server e dentro dela rode o comando abaixo para iniciar uma nova aplicação Node.js zerada.

Instale neste projeto as seguintes dependências: [WS](https://www.npmjs.com/package/ws), [DotEnv](https://www.npmjs.com/package/dotenv), [CORS](https://www.npmjs.com/package/cors), [Morgan](https://www.npmjs.com/package/morgan), [Helmet](https://www.npmjs.com/package/helmet) e [Express](https://www.npmjs.com/package/express). O Express é opcional para construção de um servidor websocket, mas como é um web-framework muito popular, vou usá-lo nos exemplos também. Sem Express você pode subir o seu websocket server em um server HTTP comum do Node.js. Junto do Express, decidi por instalar algumas dependências a mais que explico mais tarde, mas que também são opcionais.

|  | 

npm  i  express ws dotenv cors helmet morgan



 |

Agora crie nesta pasta três arquivos:

*   index.js: responsável por subir nosso servidor;
*   app.js: arquivo de configuração da aplicação Express;
*   app-ws.js: arquivo de configuração do servidor de WebSockets;
*   .env: arquivo de variáveis de ambiente

No arquivo .env, você definirá as variáveis de ambiente da sua aplicação, ou seja, as configurações mais gerais, geralmente relacionadas à infraestrutura e segurança. Em um primeiro momento, vamos precisar de apenas uma configuração: PORT, que é a porta de rede onde nosso servidor estará escutando requisições vindas dos clientes. Caso tenha instalado o pacote CORS, adicione uma configuração para ele também.

|  | 

#.env

PORT=3000

CORS_ORIGIN=*



 |

Usarei aqui a porta 3000, que é muito popular para aplicações Node.js em geral, mas funciona em qualquer porta que você definir e estiver disponível. Para o CORS, coloquei * a título de exemplo, mas aqui você deve colocar o endereço HTTP do front-end que estará autorizado a acessar o seu servidor, como uma de várias medidas de segurança que você pode definir. Asterisco é a mesma coisa que nada.

Como mencionei antes, o Express é opcional. No entanto, ele permite que você tenha funcionalidades bem interessantes no seu servidor de websockets como por exemplo uma rota de login antes de um socket ser estabelecido. Abaixo, o meu app.js de exemplo com o Morgan configurado para logs de acesso junto do CORS e do Helmet, para segurança básica de servidor.

| 

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22



 | 

//app.js

const  express  =  require('express');

const  cors  =  require('cors');

const  helmet  =  require('helmet');

const  morgan  =  require('morgan');

const  app  =  express();

app.use(cors({  origin:  process.env.CORS_ORIGIN  ||  '*'  }));

app.use(helmet());

app.use(express.json());

app.use(morgan('dev'));

app.post('/login',  (req,  res,  next)  =>  {

res.json({  token:  '123456'  });

});

module.exports  =  app;



 |

Repare que nossa rota de login sempre devolve um token, já que implementar alguma mecanismo de autenticação está fora do escopo deste tutorial. Se quiser aprender a implementar um profissional, recomendo ler o tutorial de [JSON Web Token](https://www.luiztools.com.br/post/autenticacao-json-web-token-jwt-em-nodejs/).

Agora vamos no nosso arquivo index.js e vamos apenas subir o nosso server Express nele, nada de websockets por enquanto.

|  | 

//index.js

const  app  =  require('./app');

const  server  =  app.listen(process.env.PORT  ||  3000,  ()  =>  {

console.log(`App Express is  running!`);

})



 |

Com isso, agora temos o bastante para subir um servidor web que recebe requisições POST em uma rota de login no endereço localhost:3000.

Vamos só ajustar nosso package.json para que incluir um script de inicialização que carrega as variáveis de ambiente e executa nosso index.js

|  | 

//package.json

"scripts":  {

"start":  "node -r dotenv/config index"

},



 |

Agora basta executar a aplicação no terminal (primeiro navegue até a pasta do projeto) e testar nossa rota de login usando [Postman](https://www.postman.com/) ou [Insomnia](https://insomnia.rest/download).

![](https://www.luiztools.com.br/wp-content/uploads/2021/07/Captura-de-Tela-2021-07-18-às-12.39.14.png)

O setup inicial do nosso projeto está completo!

[![](https://www.luiztools.com.br/wp-content/uploads/2021/01/banner-fullstack.jpg)
](https://www.luiztools.com.br/curso-fullstack?utm_source=blog&utm_medium=link&utm_campaign=auto&utm_content=curso-fullstack)  

### #2 – Setup do servidor de WebSockets

Agora que temos um servidor web up, vamos criar nosso módulo de websockets para que ele possa receber conexões deste tipo, ao invés de requisições HTTP comuns, e a partir daí iniciar a comunicação full-duplex (bi-lateral).

Pegue o seu arquivo app-ws.js para criarmos a primeira versão do nosso servidor, que explicarei a seguir.

| 

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27



 | 

const  WebSocket  =  require('ws');

function  onError(ws,  err)  {

console.error(`onError:  ${err.message}`);

}

function  onMessage(ws,  data)  {

console.log(`onMessage:  ${data}`);

ws.send(`recebido!`);

}

function  onConnection(ws,  req)  {

ws.on('message',  data  =>  onMessage(ws,  data));

ws.on('error',  error  =>  onError(ws,  error));

console.log(`onConnection`);

}

module.exports  =  (server)  =>  {

const  wss  =  new  WebSocket.Server({

server

});

wss.on('connection',  onConnection);

console.log(`App Web Socket Server is  running!`);

return  wss;

}



 |

Começamos este módulo carregando a dependência do pacote WS e definimos uma série de funções que só fazem mais sentido ao final do módulo onde exportamos uma função que receberá um servidor HTTP por parâmetro, usando o Princípio de Inversão de Dependência (DIP) do SOLID. Não precisa obrigatoriamente ser feito assim, mas é uma boa prática de arquitetura.

Esta função que estamos exportando serve para configurar e retornar o nosso objeto wss (web socket server). Começamos a configuração através da classe WebSocket.Server passando o servidor HTTP recebido por parâmetro. Depois, adicionamos um handler/listener/callback (como quiser chamar) do evento connection que será disparado toda vez que uma nova conexão for efetuada com sucesso em nosso servidor de websockets.

Se você olhar mais acima, verá o que faz essa função: ela recebe o websocket (a conexão cliente-servidor) e a request HTTP original. Com este objeto websocket (ws), podemos configurar então os callbacks para os eventos de message e error.

O evento de message será disparado toda vez que o servidor receber uma mensagem do cliente. Assim, nesta function onMessage, que recebe uma string data por parâmetro, você tratará o recebimento de requests (sempre string) dos clientes de websocket. O objeto ws recebido por parâmetro é a conexão do cliente com seu servidor e pode utilizá-la para enviar mensagens de volta para ele.

O evento de error será disparado toda vez que o servidor tiver uma falha inesperada. Assim, nesta função onError, que recebe um objeto de erro você tratará as falhas do servidor e através do objeto ws recebido por parâmetro, consegue receber quem é o cliente que ocasionou o erro.

Agora, vamos plugar este app-ws.js em nosso app.js através de configurações no index.js.

|  | 

//index.js

const  app  =  require('./app');

const  appWs  =  require('./app-ws');

const  server  =  app.listen(process.env.PORT  ||  3000,  ()  =>  {

console.log(`App Express is  running!`);

})

appWs(server);



 |

Repare como peguei o retorno do listen do app Express, que nada mais é do que um server HTTP e passei para a função exportada do app-ws. Com isso, ao rodarmos agora nossa aplicação novamente com npm start, não apenas teremos subido um server HTTP mas também um server WS, na mesma porta. Conforme o protocolo utilizado na requisição, HTTP ou WS, a aplicação específica será chamada.

Mas como eu testo um servidor de websockets sem ter uma cliente construído?

[![](https://www.luiztools.com.br/wp-content/uploads/2021/06/curso-nodejs-2.jpeg)
](https://www.luiztools.com.br/curso-nodejs?utm_source=blog&utm_medium=link&utm_campaign=auto&utm_content=curso-nodejs)  

### #3 – Testando o WebSocket Server

Para fazer os testes do websocket server precisamos de um cliente de websockets. Podemos criar um com algum pacote como Socket.io ou podemos, no mesmo estilo do Postman, usar um client gráfico como o Smart WebSocket Client, disponível para download gratuito como extensão do Google Chrome [neste link](https://chrome.google.com/webstore/detail/smart-websocket-client/omalebghpgejjiaoknljcfmglgbpocdp). Basta instalar ele como extensão e deixá-lo ativo no navegador.

Certifique-se de estar com seu servidor de websockets rodando e configure o endereço do Smart WS Client para ws://localhost:3000, que é o endereço do nosso servidor. Clique em Connect e se tudo deu certo, ele deve avisar “everything is ok”, bem como se você olhar o terminal onde está rodando o servidor, verá uma mensagem no console de que ele passou pela função onConnection.

![](https://www.luiztools.com.br/wp-content/uploads/2021/07/Captura-de-Tela-2021-07-18-às-14.17.05.png)

Agora, você pode usar a caixa de texto e o botão sendo do Smart WS Client para enviar mensagens para o seu servidor. Como não implementamos nenhuma lógica específica nele, você apenas verá no terminal do backend as mensagens chegando, indicando que o servidor as está recebendo, bem como receberá uma mensagem “recebido” como retorno no seu cliente.

Em um cenário real, provavelmente você irá querer trabalhar com JSON ao invés de mensagens de texto plano, e também armazenar no servidor quem é quem dentre todos os clientes conectados. Mas a lógica sempre será essa aí que acabei de mostrar.

Além disso, repare que usei o protocolo ws:// no início do endereço. WebSockets que estejam rodando em canal HTTPS devem usar o protocolo wss:// para funcionar.

E por fim, na próxima lição vamos aprender a enviar mensagens do servidor para os clientes, bem como iremos adicionar segurança em nosso websockets server, para que somente usuários autenticados possam se conectar nele. Você confere a parte 2 clicando [neste link](https://www.luiztools.com.br/post/como-criar-um-servidor-de-websockets-em-node-js-2/).

[![](https://www.luiztools.com.br/wp-content/uploads/2021/05/banner-curso-bot.jpg)
](https://www.luiztools.com.br/curso-beholder?utm_source=blog&utm_medium=link&utm_campaign=auto&utm_content=curso-beholder)