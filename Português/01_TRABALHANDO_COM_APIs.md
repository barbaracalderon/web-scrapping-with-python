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

| GET                                                                | POST                                               |
|--------------------------------------------------------------------|----------------------------------------------------|
| Obter dados do servidor                                            | Alterar o estado ou mandar informação sigilosa     |
| Pode deixar uma página marcada pra acesso futuro, daí tu pega dali | Os parâmetros são adicionados em um corpo separado |
| Os parâmetros são adicionadoos diretamente na URL                  | `r = requests.post(url, params=data)`              |
| _Não deve ser usado para informações sensíveis_                    | -                                                  |

Se tudo der certo com a sua requisição, você deve receber um status code 200. Se as coisas forem erradas, você recebe o código 404 de erro de página não encontrada.

# JSON

O nome vem de "JavaScript Object Notation", ou Notação de Objeto em JavaScript, mas hoje em dia o JSON é indepentende da linguagem. Não tem mais a ver com Javascript.

JSON é o formato padrão de troca de dados na web.

Quando o cliente envia uma requisição API para o servidor, o mesmo responde enviando o dado no formato JSON. O mesmo acontece com as requisições de método POST - a resposta tá no formato JSON.

O formato JSON é criado sobre duas estruturas da programação fundamentais: **dicionários** e **listas**. 

Arquivos JSON podem crescer no caos com a quantidade de dados MAS, é por isso que a documentação do API é importante: você basicamente pega só o pedaço de informação que precisa, em vez de tudo. Isso porque você tá procurando algo bem específico, e dá pra se basear na documentação do API para achar.

# Biblioteca Pandas

À essa altura, é muito útil conhecer o Pandas. É uma biblioteca open-source criada por cimma da linguagem Python bastante rápida e flexível, sendo boa para análise de dados e manipulação. Pandas funciona bem com dicionários e listas.

Aqui tem uma [introdução ao Pandas](https://pandas.pydata.org/pandas-docs/stable/user_guide/10min.html).


Os dois objetos mais importantes no Pandas são **DataFrame** e **Series**. O DataFrame pode ser visto como uma tabela completa; e o Series pode ser visto como uma coluna.

# Paginação API

Quando tu envia uma requisição ao Google para uma busca, o resultado tem (algumas vezes) mais de um milhão de páginas. Seu computador não vai fazer o download de todos os resultados de uma vez. No lugar disso, o Google vai colocar alguns resultados na página 1, outros resultados na página 2, mais um pouco de resultado na página 3, e assim vai... quando tu clica na página, ele carrega os resultados daquela página específica pra ti.

Esse é um conceito importante: a paginação de APIs. É a segmentação do conteúdo em páginas.

Vamos usar a API do GitHub Jobs como exemplo nessa questão. Código abaixo com comentários.

```python
import requests
import json

base_site = 'https://jobs.github.com/positions.json'
info = {
    "description": "data science", 
    "location": "los angeles"
}

r = requests.get(base_site, params=info)

r.status_code   # 200
r.json()        # Retorna uma lista com um dicionário dentro
len(r.json())   # 4 resultados

# A procura deve ter sido muito exclusiva então só tivemos 4 resultados
# Vamos consertar isso retirando os parâmetros que colocamos

r = requests.get(base_site)
r.ok                # True
len(r.json())       # 50 (muitos resultados!)

# Agora, vamos procurar na "próxima página", porque esse
# API trabalha com "paginação"

r = requests.get(base_site, params={"page": 2})
r.ok                # True
len(r.json())       # 50 (mas na página 2!)

# Se a gente procurar em uma página que não existe,
# a maioria das APIs vai retornar uma lista vazia

# Vamos extrair os resultados de VÁRIAS PÁGINAS

results = []
for i in range(5):
    r = requests.get(base_site), params={"page": i+1})
    if len(r.json()) == 0:
        break
    else:
        results.extend(r.json())

len(results)        # 250
```

# Exemplo API EDAMAM

Faça o seu cadastro gratuito no EDAMAM API e pegue seu ID e sua KEY de acesso. O próximo exemplo de código abaixo é sobre essa API.

```python
import requests
import json

APP_ID = "34539879384029U"                      # Só um exemplo
ADD_KEY = "c984y39c3984539470jwiu392739"        # Só um exemplo

api_endpoint = 'https://api.edamam.com/api/nutrition-details'
url = api_endpoint + '?app_id=' + APP_ID + '&app_key=' + APP_KEY

# A documentação diz algumas coisas que nós vamos usar, então
# se tem algo que não dá pra entennder, provavelmente está na documentação da API

headers = {
    "Content-type": "application/json"
}

recipe = {
    "title": "Cappucino", 
    "ingr": ["18g ground espresso", "150ml milk"]
}

# Enviando a requisição para um capuccino
r = requests.post(url, headers=headers, json=recipe)
r.status_code       # 200

capp_info = r.json()
print(json.dumps(capp_info, indent=4))     # Retorna um JSON
capp_info.keys()
print(json.dumps(capp_info["totalNutrients"], indent=4))   
# Informação de quantidade de diferentes ingredientes

# Vamos transformar em uma forma tabular (tabela)
import pandas as pd

pd.DataFrame(capp_info["totalNutrients"])
# A linha de cima mostra o formato tabela, bom para ler...
# Mas está muito largo, vamos consertar

# Mostra uma tabela mais alinhada verticalmente:
capp_nutrients = pd.DataFrame(capp_info["totalNutrients"]).transpose()

# Exportar para arquivo CSV:
capp_nutrients.to_csv("Cappucino_nutrients.csv")
```
