# Apresentando o Pacote Requests-HTML

O combo "Beautiful Soup" + "Requests" que a gente tem usado até agora pode ser inadequado para projetos grandes ou sites em Javascript. Existe uma outra biblioteca interessante chamada "Requests-HTML" **criada pelo mesmo autor da Biblioteca Requests**. Esse pacote foi criado com o objetivo de substituir e simplificar o combo. E o que é mais legal: **suporta Javascript** - o que significa que consegue executar o código JS e parsear a página resultado.

Aqui está a [documentação do Requests-HTML](https://docs.python-requests.org/projects/requests-html/en/latest/).

As anotações estão abaixo.

```python
from requests_html import HTMLSession

session = HTMLSession()
r = session.get(url='https://pt.wikipedia.org/wiki/The_Football_Association')
r.status_code               # 200

parsed_object = r.html  # Aqui o objeto parseado

# Obtendo todos os links (relativos) da página:
urls = r.htmls.links
type(urls)              # set

# Obtendo todos os links (absolutos) da página:
full_path_urls = r.htmls.absolute_links
type(full_path_urls)    # set

# PS: Nenhum dos dois retorna uma lista acima, ambos retornam o tipo 'set'

# Procurando elementos

links = r.html.find('a')        # Retorna uma lista de elementos (se comporta como o 'find_all()' do bs4)
links[4]                        # <Element 'a' class=('mw-jump-link',) href='#mw-head'>
links[4].html                   # <a class="mw-jump-link" href="#mw-head">Jump to navigation</a>
links[4].text                   # 'Jump to navigation'
links[4].attrs                  # {'class': ('mw-jump-link',), 'href': '#mw-head'}

# Procurando por tags que contenham uma determinada 'string'
# Encontre todas as tags que contenham a string 'wikipedia':
r.html.find('a', containing='wikipedia')

# Encontre todos os textos associados a tags que contenham a string 'wikipedia',
# e guarde em uma lista:
list_of_tags = [tag.text for tag in r.html.find('a', containing='wikipedia')]

r.html.find('p', first=True)        # Similar ao método 'find()' do bs4
```

## Procurando por Texto baseado em um Padrão

Se você precisa procurar por uma parte de texto que está entre duas determinadas frases, você pode capturar essa parte usando o Requests-HTML. Existe um método chamado 'search' que faz isso.

Como um exemplo, vamos tentar achar uma parte de um texto que está entre as palavras "known" e "soccer".

```python
# HTML.search
r.html.search("known{}soccer")[0]
# As chaves dizem "estamos procurando por um texto que, quando
# substituído aqui, dá um match na nossa string."
# Para pegar o texto, nós escolhemos o primeiro elemento -> ele retorna
# uma string válida.

# HTML.search_all
r.html.search_all("known{}soccer")[0]
# Retorna uma lista de textos que dão match na nossa string.
# PS: Às vezes ele pode retornar umas tags dentro; isso acontece
# porque o método search e search_all fazem um parsem no HTML cru
```
## Seletores CSS

CSS é o Cascading Style Sheets. Os seletores CSS são apenas texto que se referem a um ou mais elementos no documento HTML. 

```python
# 1) SELECIONAR ELEMENTOS BASEADO NO ID (#) ---------------------
# (Fazem a diferença entre maiúscula e minúscula)
r.html.find("#Name")    # -> Retorna uma lista
r.html.find("#name")    # -> Retorna uma lista 
r.html.find("#Duration_and_tie-breaking_methods", first=True)   # -> Retorna um elemento

# 2) SELECIONA ELEMENTOS BASEADOS EM UMA CLASSE (.) ------------
# (Dá pra pesquisar uma classe dentro de outra classe)
r.html.find(".mw-headline")             # -> Retorna uma lista
r.html.find(".metadata.plainlinks")     # -> Retorna uma lista
# A linha de cima acha uma classe (plainlinks) dentro de outra
# classe (metadata)

# 3) SELECIONA BASEADO EM OUTROS ATRIBUTOS ---------------------
# A. [atributo]: seleciona todas as tagas que tem o atributo
# B. [atributo=valor]: seleciona todas as tags com esse valor
# específico do atributo mencionado
# C. [atributo*=valor]: atributo contém a SUBSTRING "valor"

# A. 
r.html.find("[target]")                 # -> Retorna uma lista

# B.
r.html.find("[role=note]")              # -> Retorna uma lista

# C.
r.html.find("[href*=wikipedia]")        # -> Retorna uma lista

# 4) COMBINANDO FILTROS DIFERENTES JUNTOS ---------------------
# Filtrar apenas as anchor tags que tenham wikipedia no endereço:
r.html.find("a[href*=wikipedia]")
# Filtrar anchor tags com a classe "internal":
r.html.find("a.internal")
# Todas as tags div com a classe "thumb" e "tright":
r.html.find("div.thumb.tright")
# Usar o formato de chaves:
r.html.find("div[role=note][class='hatnote navigation-not-searchable']")
# -> linha de cima: nós precisamos mencionar todas os valores
# da classe que desejamos

# 5) INCORPORANDO A HIERARQUIA DE TAG ------------------------
# PS: Os seletores CSS também permitem que a gente escolha
# elementos baseados em seus pais ou irmãos
# Filtrar todas as span tags dentro de uma tag h2: você precisa
# separar ambos dentro do método find
r.html.find('h2 span')
# Filtrar o elemento que é diretamente filho de outro
# (usar o símbolo de 'maior que' >):
r.html.find("div > p")
```

## Extraindo dados gerados por Javascript

No requests-HTML, tem um método especial chamado `.render()`: ele executa o código Javascript da mesma forma que um navegador faria. Esse JS adicional pode ser armazenado e inspecionado como normalmente faríamos. Então, a única diferença para o scraping normal é que precisamos usar o método 'render' logo antes da extração dos dados.

```python
from requests_html import AsyncHTMLSession

session = AsyncHTMLSession()

r = await session.get("https///www.reddit.com/")
print(r.status_code)        # 200

divs = r.html.find('div')
links = r.html.find('a')
urls = r.html.absolute_links         # 'absolute_links' é um atributo

await r.html.arender()
# O comando acima vai fazer o download e a instalação do Chromium
# (se não tiver na máquina!). O Chromiu é um programa que se comporta
# como um navegador mas não tem uma interface gráfica

new_divs = r.html.find('div')       # tamanho maior agora
new_links = r.html.find('a')        # tamanho maior agora
new_urls = r.html.absolute_links    # tamanho maior agora

# set1.difference(set2)
# -> ele retorna apenas os elementos de set1 que não estão
# contidos no set2

new_urls.difference(urls)
# a repsosta são todas as URLs que foram geradas pelo Javascript :)

session.close()
```

Sempre que usamos o método 'render', pode levar um tempo pra funcionar. Mas você pode ajustar o parâmetro 'wait' para a quantidade de tempo (em segundos) que o Chromium vai esperar antes de carregar a página. Você também pode mudar o parâmetro 'retries' - por padrão, o Chromium vai tentar carregar a página 8 vezes.

Chegamos ao fim.

Boa sorte.

:-)