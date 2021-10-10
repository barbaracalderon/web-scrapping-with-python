# Extração de Dados em HTML

Para poder extrair dados de documentos HTML, precisamos inspecionar o código-fonte HTML. Ele vai dizer **onde** o dado está localizado no HTML. Isso é importante porque precisamos saber à qual tag e atributo o dado está atrelado. As tags em HTML dizem a categoria do dado, como, por exemplo, a tag `<img>`... que nos informa que a categoria é imagem. Mas isso sozinho pode não ser suficiente: talvez a gente precise especificar mais informações nessa tag. É pra isso que servem os atributos também. É útil saber um pouco mais sobre HTML porque é lá nesse documento que iremos inspecionar para encontrar onde o dado desejado está localizado. A gente usa isso para extraí-lo.

As tags mais conhecidas para Web Scraping são **class** e **id**. O atributo "class" é usado para agrupar elementos da mesma categoria de modo que sejam manipulados todos de uma vez; o atributo "id" é único a cada elemento.

Aqui algumas tags que vamos encontrar com alguma frequência.

* `<head>`
* `<title>`
* `<metadata>`
* `<body>`
* `<div>`*
* `<span>`*
* `<a href="">`
* `<iframe>`
* `<img src="" alt="">`
* `<ul>`
* `<ol>`
* `<li>`
* `<table>`
* `<tr>`
* `<th>`
* `<td>`
* `<style>`
* `<script>`

Infelizmente, alguns conteúdos de uma página podem apenas ser gerados por meio do Javascript. Isso significa que às vezes pode ser um desafio pra extrair o dado... mas tem formas de lidar com isso.

### Encodings

A gente lida com padrões de "encodings" de caracteres. Encodings são as formas que se pode atribuir valores legíveis por uma máquina aos símbolos, letras e caracteres no geral. Um dos primeiros padrões de encodings foi o ASCII, popular nos Estados Unidos, mas esse encoding era inadequado pra representar a maioria dos símbolos e outros idiomas. Hoje o padrão mais popular é o Unicode, mas ainda memória pode ser desperdiçada devido a símbolos não utilizados. Por isso usamos o UTF-8.

Negócio é o seguinte: precisamos compreender encondings porque alguns documentos usam encodings específicos e isso afeta a leitura do documento. A gente deve ser capaz de mudar os encondings pra que as ferramentas consigam "ler" os símbolos de modo adequado.

# Beautiful Soup

Agora a gente sabe o que é Web Scraping: é o processo de extração automática de dados de websites. Como que faz isso? Obtendo o código-fonte e processando ele para pegar os elementos desejados. **Beautiful Soup é uma biblioteca em Python para a extração de dados em documentos HTML**. É uma das ferramentas mais simples e fáceis de fazer isso.

Aqui a documentação para o [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/bs4/doc.ptbr/). Em português.

E qual é a rota de fazer Web Scraping? Quais são os passos para extrair dados da web? São basicamente cinco passos. Com exceção do primeiro passo, todo o restante é feito por meio da criação de um código em Python.

1. Inspecionar a página
2. Obter o HTML
3. Escolher um parser
4. Criar um objeto do Beautiful Soup
5. (Opcional) Exportar o HTML para um arquivo

### 1. Inspecionar a página

Esse é o passo número um. Ao usar o "developer's tool" do navegador para inspecionar a página, nós precisamos saber **onde** o nosso dado está localizado no HTML. Lembra das tags e atributos? Com quais tags e atributos o nosso dado está relacionado? É por isso que inspecionamos a página. 

Mas cuidado.

O navegador faz um monte de coisas pra gente, usuários. Às vezes, ele recebe conteúdos do servidor e o "developer's tool" automaticamente roda o código Javascript (se tiver algum) que, por sua vez, nos presenteia com algo que pode ser diferente do "HTML original" cujo conteúdo bruto (não rodado) foi enviado pelo servidor. Quando a gente analisa as coisas, a gente tá lidando com uma resposta bruta do servidor, o que significa que nenhum Javascript vai ser rodado aqui. Por isso rolam as diferenças. 

O ato de **inspecionar a página é para pegar a ideia geral de como a página está estruturada**, o que é essencial para extrair o dado que queremos.

### 2. Obter o HTML

Isso aqui é parecido com a chamada aos APIs no arquivo `01_TRABALHANDO_COM_APIS.md` nesse repositório. Isso significa que nós vamos precisar enviar uma requisição `request` para o servidor usando a **biblioteca requests**. 

Olha o código abaixo.


```python
import requests                         # Aqui a biblioteca requests

url = 'https://www.google.com'          # Só uma URL de exemplo

# Agora a gente quer mandar uma requisição com método GET para o servidor
# localizado na URL mencionada, e ela vai dar uma resposta pra gente, que 
# a gente vai alocar na variável "response"

response = requests.get(url)            

# Nós usamos o método GET porque não estamos tentando logar na página, com
# login e senha, nem nada. Se a gente tivesse fazendo isso, deveríamos usar
# o método POST no lugar do GET... porque o POST altera o estado lá no servidor.
# Mas o método GET não, ele apenas pega algo lá no servidor sem alterar nada.

# Tem muita informação dentro dessa variável "response"
print(response.status_code)         # Retorna 200 se tudo ok
print(response.ok)                  # Retorna um booleano
print(response.content)             # Ele printa o documento HTML

# Mas manipular todo o conteúdo HTML em uma única string é... chato
# demais. Por isso que "parsear" é quase obrigatório: ele divide a
# stirng em componentes sintáticos. Isso significa que ele vai 
# identificar todos os componentes HTML, suas relações uns com os
# outros, seus atributos e conteúdos. 

# E aí algo massa acontece: uma árvore parse é criada.

# É por isso que a gente faz um parse no response.content
```

### 3. Escolher um parser

Dá pra pensar numa árvore parse como uma árvore família invertida de elementos HTML. Árvore como na estrutura de dados árvore. 

Quem cria essas árvores?

O parser. Ele é um programa, ou algoritmo, desenhado para "parsear", para criar a árvore parse. O Beautiful Soup não "parseia" o código, ele na verdade usa um "parser" externo... e nós temos que indicar qual parser usar. São três: `html.parser; lxml; html5lib`. 

O Beautiful Soup vê esses três parseadores da seguinte forma: ele diz que o html.parser é o pior, mesmo que seja um parseador criado em Python. É considerado o pior porque ele às vezes não sabe lidar com erros, o que gera inconsistências. O melhor é o lxml, que é o mais rápido e sabe lidar com erros. O do meio é o "html5lib", mesmo sendo o mais devagarzinho - ele consegue lidar com erros e parseia o HTML da mesma forma que um navegador faz.

Se tu não escolher um parser, o Beautiful Soup escolhe pra ti. Mas isso não é massa - é melhor que você explicitamente escolha o parseador. 

### 4. Criar um objeto Beautiful Soup para manipular e extrair dados

Com o HTML nas mãos e a árvore parse, o Beautiful Soup cria o objeto. Esse objeto vai manipular e extrair o dado que nós quisermos dentro desse documento HTML.

```python
import requests
from bs4 import BeautifulSoup           # Importar o Beautiful Soup

url = 'https://www.google.com'
response = requests.get(url)

# Vamos criar o objeto "soup", um objeto do Beautiful Soup
# Primeiro argumento: o conteúdo da resposta do servidor em bytes
# Segundo argumento: o "parser" da nossa escolha
soup = BeautifulSoup(response.content, 'lxml')

# Poderíamos ter usado outros parsers:
soup2 = BeautifulSoup(response.content, 'html.parser')

# Ou até:
soup3 = BeautifulSoup(response.content, 'html5lib')

# PS: Em vez de usar response.content, poderíamos ter usado o
# response.text; a diferença é que o primeiro é em bytes, e esse
# segundo é em texto puro. Por exemplo:
soup4 = BeautifulSoup(response.text, 'lxml')

# Sempre que tiver dúvidas: printa as coisas pra tu "ver" o que elas são
```

### 5. (Opcional) Exportar o HTML para um arquivo (muito recomendado)

Apesar de ser opcional, pode ser uma boa ideia porque:

a) O código no navegador pode ser diferente daquele enviado pelo servidor pra gente. Lembra que o navegador faz um monte de coisa automática para nos presentear com o conteúdo visual da página.

b) O parser... pode não ter "parseado" o documento de modo correto. Essa é a chance de você "ver" isso.

c) Pra referência futura.

Todas as opções de cima são boas razões. Quando a gente tem um bug, inspecionar o arquivo exportado é uma boa opção pra debugar o problema. Pode salvar um monte de tempo ir logo nesse documento salvo.

## Procurando e Navegando na Árvore HTML

É massa dar uma olhada na documentação do Beautiful Soup. Tem muita coisa lá explicada em detalhes.

Ok, então agora a gente já criou o objeto Beautiful Soup e salvou na variável "soup". Como dito antes, essa variável contém todo o HTML e mais: seus atributos, valores e mais. O seu formato é a **árvore** HTML. E a gente consegue navegar essa árvore para procurar coisas.

_Árvore num sentido de Estrutura de Dados mesmo._

### Por meio dos Métodos: `.find()` e `.find_all()`

Os dois **métodos mais usados** (métodos são as funções de um objeto) do objeto Beautiful Soup são `.find()` e `.find_all()`. Como que a gente usa? **A forma mais simples é procurar por um elemento por meio do nome da sua tag**, então é isso que vai dentro do parênteses do método como argumento.

A diferença entre os dois métodos é que: o `.find()` procura pelo elemento e ele pára quando encontra o primeiro resultado; se o nome da tag não existe no documento, ele retorna `None`. Já o método `.find_all()` cria uma lista e vai armazenando dentro dessa lista cada elemento que ele encontra no meio do caminho que tenha uma tag igual. Se o nome da tag não existe no documento, ele retorna uma lista vazia. 

Também dá para, em vez de procurar o documento inteiro, procurar apenas um "pedaço" do documento - cortando-o em "porções". Assim a gente procura em uma determinada porção apenas, não no corpo todo do documento. Essas coisas podem fazer diferença em larga escala, no sentido de economizar tempo.

```python
import requests
from bs4 import BeautifulSoup

url = 'https://www.wikipedia.org'
response = requests.get(url)

soup = BeautifulSoup(response.text, 'lxml')
# A variável 'soup' de cima representa todo o documento, incluindo o conhecimento
# sobre os elementos e seus atributos (então não é apenas texto contido ali)

anchor = soup.find('a')
anchors = soup.find_all('a')
# (anchors = âncoras)

# Vamos criar um objeto de uma porção da página, como uma tabela.
# (Lembra que tbody é a tag do corpo da tabela)

table = soup.find('tbody')      
type(table)     # Retorna 'bs4.element.Tag'
# A tag de cima é tratada quase da mesma forma que a variável 'soup'

# Tags filhos
print(table.contents)
# A linha de cima mostra todos os elementos-filhos do objeto table
# (esses filhos estão alocados em uma lista)

# Tag pai
# O pai de uma tag é o elemento na qual essa tag está alocada
print(table.parent)

# PS: uma tag pode ter VÁRIOS filhos mas apenas UM pai, é por isso que
# o pai é uma tag mas os filhos são uma lista. 
```



_Continua_...