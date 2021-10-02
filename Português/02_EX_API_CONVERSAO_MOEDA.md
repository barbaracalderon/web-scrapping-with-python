# Exemplo: API de Moedas

```python
# Vamos importar o "resquests" para conversar com o servidor:
import requests

# A nossa API está localizada nesta URL:
base_url = 'https://api.exchangeratesapi.io/latest'

# A resposta do servidor, guardamos na variável "response"
# Tudo está contido nessa resposta "response"
response = requests.get(base_url)

print(response.ok)              # Retorna um booleano
print(response.status_code)     # Retorna 200 se tudo ok


print(response.text)            # Em formato texto
print(response.content)         # Em formato de bytes

# Mas essa API também fornece uma resposta em formato JSON
jsonr = response.json()
print(type(jsonr))              # Retorna um dicionário (dict)
# Lembra que "dict" é um estrutura de dados do Python
```

É importante saber lidar com dados json. Pra fazer a nossa vida ficar mais fácil, vamos importar "json", um pacote Python.

```python
import json

# Dois métodos mais importantes em JSON: loads e dumps
# - .loads(string): converte uma string formata em JSON para um objeto Python
# - .dumps(obj): converte um objeto Python de volta pra uma string

# Apresentar o dado com uma identação de 4 espaços:
print(json.dumps(response.json(), indent=4))

# Te diz as chaves do dado JSON:
response.json().keys()
# dict_keys(['rates', 'base', 'date'])
```