# EN: Application Programming Interface (API)

It specifies how a software component should interact. It's like a contract between a client and a server: if a client request a specific format, the server will always respond in a documented format or initiate a defined action.

We deal with web-based APIs.

They are extensively used to incorporate a third-party functionality to your service. APIs can be free or paid. All APIs should have some form of documentation.

You connect to an API through an HTTP request. First things, first.

"HTTP" is protocol. More specifically, it's the **HyperText Transfer Protocol**. It basically specifies **how** requests and responses (between client and server) are to be formatted and transmitted. This is how all the surface Internet works. Everything you see on the surface web is a response from a server, which is a computer machine responsible for storing files. Your computer is the client.

When your computer makes a request to that server. The server gives you a response: it sends all the files to your computer... which, in turn, downloads it temporarily and builds the webpage with the help of the browser. The browser, actually, connects all the files together in order to give the purposed visual representation of them. The web page.

The two most popular HTTP requests are **GET** and **POST**.

GET | POST
--- | ----
Obtain data from a server | Alter state or send confidential information
Can be bookmarked |Parameters are added in a separate body
Parameters are added directly to the URL | -
_Not used for sensitive information_ | -


If everything is fine with your request, you should get a 200 status code. If things go wrong, you get the 404 status code error of page not found.

# JSON

The name comes from "JavaScript Object Notation" but nowadays it is language-independent. It's got nothing to do with Javascript anymore.

JSON is the standard format for data exchange in the web.

When the client sends an API request to the server, it responds by sending the data in the JSON format. It also works with POSTs requests - the response is in the JSON format.

The JSON format is built upon two fundamental programming structures: **dictionary** and **list**.

JSON files can grow into a chaos of data BUT, that's why the API documentation is important: you'll basically only get a glimpse of all the volume of information. That's because you'll be searching for very specific things, and you can rely on the documentation to find it.

---

# PT: Application Programming Interface (API)

Ou Aplicação de Interface de Programação.

O API especifica como oo componente de software deve interagir. É como um contrato entre o cliente e o servidor: se um cliente requisita um formato específico, o servidor sempre vai responder em um formato documentado ou iniciar uma ação definida. 

A gente lida com APIs baseados na web.

Eles são extensivamente usados para incorporaçção de funcionalidades de terceiros nos nossos serviços. Os APIs podem ser gratuitos ou pagos. Todos os APIs devem vir com algum tipo de documentação.

Você conecta ao API por meio de uma requisição (request) em HTTP. Vamo com calma.

"HTTP" é um protocolo. Mais especificamente, é o **HyperText Transfer Protocol**. Ou, Protocolo de Transferência de HiperTexto. Ele basicamente especifica **como** requisições e respostas (entre cliente e srevidor) devem ser formatados e transmitidos. É assim que a internet de superfície funciona. Tudo que você vê nela é uma resposta de um servidor, que é uma máquina computador responsável por guardar arquivos. Seu computador é o cliente.

Quando seu computador faz uma requisição àquele servidor, ele te dá uma resposta: ele manda todos os arquivos pedidos ao seu computador... que, por sua vez, faz o download disso temporariamente e constrói a página web com a ajuda do navegador. O navegador, na verdade, conecta todos os arquivos juntos pra conseguir montar a representação visual que era o objetivo de tudo. A página web.

As duas requisições HTTP mais populares são **GET** e **POST**.

Get significa "pegar".

Post significa "postar".

GET | POST
--- | ----
Obter dados do servidor| Alterar o estado ou mandar informação sigilosa
Pode deixar uma página marcada pra acesso futuro, daí tu pega dali | Os parâmetros são adicionados em um corpo separado
Os parâmetros são adicionadoos diretamente na URL | -
_Não deve ser usado para informações sensíveis_ | -

Se tudo der certo com a sua requisição, você deve receber um status code 200. Se as coisas forem erradas, você recebe o código 404 de erro de página não encontrada.

# JSON

O nome vem de "JavaScript Object Notation", ou Notação de Objeto em JavaScript, mas hoje em dia o JSON é indepentende da linguagem. Não tem mais a ver com Javascript.

JSON é o formato padrão de troca de dados na web.

Quando o cliente envia uma requisição API para o servidor, o mesmo responde enviando o dado no formato JSON. O mesmo acontece com as requisições de método POST - a resposta tá no formato JSON.

O formato JSON é criado sobre duas estruturas da programação fundamentais: **dicionários** e **listas**. 

Arquivos JSON podem crescer no caos com a quantidade de dados MAS, é por isso que a documentação do API é importante: você basicamente pega só o pedaço de informação que precisa, em vez de tudo. Isso porque você tá procurando algo bem específico, e dá pra se basear na documentação do API para achar.
