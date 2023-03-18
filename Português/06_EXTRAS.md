# Anotações Extras

Essas são as minhas anotações de estudo do curso ["Web Scraping and Crawling with Python: Beautiful Soup, Requests & 
Selenium"](https://www.udemy.com/course/web-scraping-with-python-beautifulsoup/) do instrutor Waqar Ahmed.

## Escrevendo e lendo de arquivos CSV

Antes de mais nada, um workbook é a mesma coisa que abrir um arquivo CSV com todas as abas que ele possa ter. Em 
segundo lugar, um "sheet" (ou planilha) é a mesma coisa que uma aba dentro do workbook. O instrutor faz essa 
diferenciação. Dá para ter várias abas dentro de um mesmo workbook.

# Escrevendo: xlsxwriter

Esse é o nome do módulo. Você deve importá-lo e trabalhar com ele. Da seguinte forma:

```python
from xlsxwriter import Workbook

# O próprio objeto
workbook = Workbook("first_file.xlsx")

# Uma planilha denro do workbook
worksheet = workbook.add_worksheet()

worksheet.write(0, 0, 'zero row and zero column')
worksheet.write(0, 1, 'zero row and one column')
worksheet.write(1, 0, 'one row and zero column')
worksheet.write(1, 1, 'one row and one column')

workbook.close()
```

Se você escreveu o código acima, e fez a instalação correta do módulo xlsxwriter, você deve conseguir criar um 
arquivo XLSX e salvar com conteúdo escrito dentro dele. 

Dá para fazer um pouco mais: dá para brincar com loops (laços) ao seu favor. Da seguinte forma:

```python
from xlsxwriter import Workbook

workbook = Workbook("second_file.xlsx")

for row in range(0, 20):
    worksheet.write(row, 0, "Row Number")
    worksheet.write(row, 1, row)

workbook.close()

# O código acima cria um arquivo .xlsx e o loop fica responsável por escrever nas primeiras 20 linhas.
# Cada linha contém a string "Row Number" na primeira coluna, e na segunda coluna o número da linha (int)
```

### Lendo: xlrd

Para a leitura, usamos o módulo xlrd. Veja o código abaixo.

```python
import xlrd

workbook = xlrd.open_workbook("second_file.xlsx")

# Pega uma planilha específica (a primeira) dentro do workbook
worksheet = workbook.sheet_by_index(0)

# Printa o número das linhas escritas na planilha
print(worksheet.nrows) 

# Lê as linhas e os seus valores (linha, número)
for row in range(rows):
    first_column, second_column = worksheet.row_values(row)
    print(f'{first_column}    {second_column}')
```

## Criando um user-agent falso

### fake_useragent

O user-agent é um software que age como se fosse eum usuário. De modo resumido, a gente fala para o servidor que um 
navegador é que está fazendo algum trabalho nele... e não um pedaço de código. É uma forma de simular um agente real 
usando navegador. 

```python
from fake_useragent import UserAgent
import requests

user_agent = UserAgent()

header = {'user_agent': user_agent.chrome}

# O timeout diz ao código para não esperar mais do que tantos segundos por uma response
page = requests.get('https://www.google.com', headers=header, timeout=6)
print(page.content)
```

# Regular Expressions (REGEX)

É útil para dar match em caracteres, encontrar tags ou dados usando a árvore de parse (como no Beautiful Soup, por 
exemplo). 

O lado positivo é que é bastante rápido e eficiente, contanto que saibas o que está procurando e como criar a 
expressão. O lado negativo é que, quanto mais complexa for a sua expressão, mais complexa a string de busca vai se 
tornar - o que reduz a legibilidade. 

Eu encontrei esse tutorial do Corey Schafer bem útil e fácil de entender - [Corey Schafer - Regular 
Expressions (Regex) Tutorial: How to Match Any Pattern of Text.](https://www.youtube.com/watch?v=sa-TUpSx1JA) O 
porém é que se encontra na língua inglesa. Eu curto bastante o trabalho do Corey e acho ele bem talentoso nas suas 
explicações. Talvez queira dar uma olhada no canal dele e se inscrever, tem bastante conteúdo Python e uns achados 
bem bons. 

A [documentação sobre expressões regulares (re)](https://docs.python.org/3/library/re.html) pode ser achada nesse link.

Dá para ser bem útil se trabalhada com cuidado. Eu sugiro dar uma olhada nos vídeos do Corey Schafer. 

# O Conversor cURL

*Essas anotações são de trabalhar com essa ferramenta no dia a dia*. 

Em vez de trabalhar aos poucos, fazendo uma busca de coleta de informações sobre headers, cookies, data, json, 
params e mais... dá para usar o comando `cURL` ao nosso favor. 

Funciona da seguinte forma. 

Vamos assumir que queremos chegar na response do site do pudim.com.br (nada demais aqui, só uma imagem de um pudim). 
Como podemos traduzir essa ação bem sucedida em um código Python usando o cURL?

1. Abra o navegador e aperte F12 para abrir o dev tools
2. Digite a URL e pressione Enter
3. Vá para a aba de Rede, dentro do dev tools, e procure por status code 200 e documento html
4. Clique nessa linha e depois em `copiar como cURL`
5. Vá para o [cURL converter](https://curlconverter.com/python/) e procure por Python
6. Cole o conteúdo copiado
7. Vai aparecer o código Python que alcança a response desejada do pudim
8. Pronto

Esse é um atalho maravilhoso.

