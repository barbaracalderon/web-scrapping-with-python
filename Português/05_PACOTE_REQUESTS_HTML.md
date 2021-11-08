# Apresentando o Pacote Requests=HTML

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