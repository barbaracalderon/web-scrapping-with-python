# Tabelas e Pandas

É hora de aprender como armazenar os dados de um modo estruturado. Pra isso, usamos a biblioteca **pandas**, que é bastante visual. 

## documentação pandas

Mais sobre a [documentação pandas aqui](https://pandas.pydata.org/docs/).

## criando um DataFrame

_Os exemplos dessa seção são do site Rotten Tomatoes conforme mostrado no curso._

Vamos supor que temos uma lista para cada informação de um filme: uma lista para os títulos dos filmes (`movie_names`); uma lista para os anos (`years`); uma lista para a pontuação dos filmes (`scores`); outra lista para as pontuações ajustadas (`final_adj`); uma lista para os diretores dos filmes (`final_directors`); uma lista para as sinopses (`synopsis_text`); uma lista para os elencos (`cast`) uma outra outra para os consensos dos filmes (`consensus_text`).

A gente vai criar um DataFrame do pandas para armazenar as informações desses filmes - isso significa "popular". Por sua vez, o DataFrame tem o formato de uma tabela. As listas serão as "colunas" da tabela e as "chaves" são os cabeçalhos de cada coluna.

```python
import pandas as pd
pd.set_option('display.max_colwidth', -1)

movies_info = pd.DataFrame()
movies_info["Movie Title"] = movies_names
movies_info["Year"] = years
movies_info["Score"] = scores
movies_info["Adjusted Score"] = final_adj
movies_info["Director"] = final_directors
movies_info["Synopsis"] = synopsis_text
movies_info["Cast"] = cast
movies_info["Consensus"] = consensus_text
```

## desfazendo informação truncada

_PS: o pandas abrevia qualquer texto que não caiba na tela. Para não deixá-lo "truncar" a informação, precisamos setar a opção: `pd.set_option('display.max_colwidth', -1)`_

## exportando dados para arquivos CSV e Excel

Agora, a análise dos dados pode ser feita. O desejado é exportar esses dados para arquivos CSV (_Comma Separated Values_) ou Excel, para poder ter enquanto arquivo. E o pandas fornece métodos simples de conversão de dataframes nesses formatos: `.to_csv() ` e `.to_excel()`. Eles criam um arquivo no formato do método na mesma pasta em que está o código Python. 

No nosso caso, é feito conforme abaixo. 

```python
# Exportando o DataFrame para um arquivo CSV:
movies_info.to_csv("movies_info.csv", index=False, header=True)

# Exportando o DataFrame para um arquivo Excel:
movies_info.to_excel("movies_info.xlsx", index=False, header=True)
```

