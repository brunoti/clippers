# Introdução ao Clojure Component. Como usar o pacote Component do Stuart… | by Renato Alencar | Medium
Como usar o pacote Component do Stuart Sierra?
----------------------------------------------

[

![](https://miro.medium.com/v2/resize:fill:88:88/1*g329iTCYf84LaqjjvnUSmg.jpeg)










](https://renatoalencar.medium.com/?source=post_page-----6ffbf388f6ae--------------------------------)

![](https://miro.medium.com/v2/resize:fit:1400/1*C2crElnGWQ3IeSS5yZ6czg.jpeg)

Se você chegou até aqui já deve saber que Clojure é uma linguagem de programação funcional baseada em Lisp que tem ganhado popularidade no Brasil nos últimos anos, principalmente por causa da Nubank. Na minha experiencia pessoal, Clojure tem sido uma linguagem com uma entrada um pouco difícil, algumas coisas são complicadas de inicio, mas você ganha tração bem fácil à medida que você vai tentando se aprofundar.

Algo que eu tenho notado é a falta de material mais profundo sobre Clojure em português, o que torna difícil para brasileiros em geral se aprofundarem no aprendizado da linguagem e na capacidade de gerar discussões em torno da linguagem e dos padrões que a comunidade cria em volta dela. Parte talvez do sucesso que o JavaScript, em especial no Brasil, tenha sido talvez devido a quantidade crescente de usuários e comunidades em português que surgem. Por isso decidi escrever esse artigo, e talvez outros, sobre como tem sido aprender Clojure, como um hobby nas horas vagas. Em especial, nesse artigo sobre a lib Component do Stuart Sierra.

Stuart Sierra costumava trabalhar na Cognitect como consultor e desenvolvedor, e analisando os problemas na forma como comumente desenvolvedores estruturavam o estado de suas aplicações, em especial coisas que tem características de singleton, como um pool de conexão com bancos de dados, caching ou partes separadas de uma aplicação, criou um pequeno framework baseado na ideia de injeção de dependência e gerenciamento de estado da orientação a objetos: O Component.

A questão é que temos o hábito de tentar encaixar tudo no modelo MVC, e tentar ver qualquer aplicação como uma extensão disso, tendo um **banco de dados**, **visualização** e seja lá o que você considere como **lógica/modelo de negocio**, sendo que na realidade as coisas estão mais para um _spaghetti_ em formato de integrações com cache, serviços de terceiros, filas de jobs, etc. Então Stuart Sierra, definiu baseado nisso um framework que poderia criar uma abstração generalizada para lidar com esse problema, onde a ideia é:

*   Criar componentes que guardem a responsabilidade de manter uma parte do estado da aplicação;
*   Definir um limite claro entre esses componentes;
*   Definir como lidar com o ciclo de vida desses componentes, cada componente tem que saber como inicializar a si próprio e e como parar;
*   Definir as dependências entre componentes de forma explícita e que respeite os limites entre os componentes e as partes da aplicação.

Tudo isso começou semana passada, comigo tentando fazer alguns experimentos com Clojure, montar um web service simples que integrasse com banco de dados e me permitisse fazer algumas operações. Você precisar conectar com o banco e manter a mesma conexão (ou um pool) para usos posteriores, afinal você não vai criar uma nova conexão com o banco toda vez que precisar responder a uma requisição. A forma mais simples de fazer isso pode ser usando um _atom_ mesmo:

Simples, né? Parece até fácil demais para ser verdade. O problema é que quando isso cresce muito, a tendência é que vai ser bem complicado saber que tem acesso à essa conexão, devido ao fato de que essa dependência é implícita, além disso o acesso é a uma variável global e mutável. O outro problema comum é a necessidade de se adicionar cada vez mais dependências externas, que podem ser acessadas por um estado global na aplicação. Stuart Sierra explica isso com detalhes e com bons exemplos em [_Clojure in the Large_](https://www.youtube.com/watch?v=1siSPGUu9JQ), onde ele conta a experiencia dele lidando com esse tipo de problema em aplicações de larga escala.

Como o problema inicial nesse exemplo é o banco, vamos começar por ele, esse na verdade é o exemplo clássico incluso no próprio README do projeto Component. Aqui, tudo se baseia em três coisas: o conexão com o banco tem um ciclo de vida, eu devo poder acessar ela na aplicação para poder consultar e inserir objetos e eu devo conseguir gerenciar isso como um estado da aplicação. Para começar vamos definir o que seria exatamente um componente banco de dados, o que devemos manter como estado e como gerenciar o ciclo de vida:

*   Um banco de dados pode ser caracterizado aqui como a URL de conexão, a conexão em si e o objeto banco de dados onde a biblioteca faz as consultas;
*   Eu preciso me conectar ao banco quando a aplicação e iniciar e guardar o banco em si e a conexão para ser fechada depois;
*   Eu preciso fechar a conexão quando a aplicação parar ou pelo menos permitir faze-lo.

Em Clojure podemos definir os dados com um Record, que é um tipo de dados que tem a implementação de _map_ e que são utilizados como tipo para guardar dados em formato de _chave-valor_.

Agora que sabemos como encapsular nosso banco de dados, precisamos encapsular o próprio ciclo de vida dele. O Component usa o conceito de Protocolos do Clojure para resolver isso, inclusive protocolo é um conceito tao poderoso por si só que daria um artigo inteiro. O protocolo em questão é o _Lifecycle_. Usando um protocolo em comum, o framework sabe como iniciar e como parar qualquer componente, assim você só tem que se preocupar com a implementação. Além de definir o componente em si, é interessante definir uma função (pura) que crie o componente, um _constructor_.

Bem, nesse momento, você talvez já teve algum tipo de epifania e pensado: "Mas o servidor web também se encaixa nesse padrão". Sim, e vamos encapsular ele também:

Aqui nós adicionamos algo novo: a função _component/using_. Ela é responsável poder deixar explicito que um componente depende de outro, nesse caso o _app_, que possui uma chave _handler._ Nesse caso o handler deve ser uma função que é responsável por responder a requisições.

Agora vamos precisar de um componente _App_, que deve depender do banco para fazer as consultas necessárias, usando esse componente vamos passar o banco de dados pelo _map_ da requisição para ser usado internamente pelo handler.

Com tudo definido, só precisamos juntar tudo em um _map_, usando a função _component/system-map_. Aqui você pode criar uma função pura que é responsável por criar o sistema em si. E então chamar _component/start_ nesse _map_, dentro da função _-main_.

Você pode estar se perguntando por que eu não usei _Compojure_ ou por que eu usei MongoDB como exemplo. Bem, a intenção desse exemplo é de ser focado no _Component_, e ao mesmo tempo como ele pode ser usado em conjunto com outras ferramentas. Eu tentei evitar ao máximo adicionar a necessidade de conhecimento sobre outras ferramentas para facilitar o entendimento do exemplo.

O código com as configurações todas no ponto de rodar tá no GitHub em [https://github.com/renatoalencar/component-example](https://github.com/renatoalencar/component-example).

*   [https://github.com/stuartsierra/component](https://github.com/stuartsierra/component)
*   Clojure in the Large: [https://www.youtube.com/watch?v=1siSPGUu9JQ](https://www.youtube.com/watch?v=1siSPGUu9JQ)
*   Components Just Enough Structure: [https://www.youtube.com/watch?v=13cmHf_kt-Q](https://www.youtube.com/watch?v=13cmHf_kt-Q&t=6s)