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

### Incorporando Parâmetros na requisição GET

Sempre leia a documentação do API para entender como incorporar parâmetros na sua requisição. No caso abaixo, a API é a de conversão de moedas que está na base_url do primeiro código desse arquivo.

```python
param_url = base_url + '?symbols=USD,GBP'
response = requests.get(param_url)

data = response.json()      # Isso aqui é um dicionário
data['base']                # 'EUR'
data['date']                # '2021-10-02'
data['rates']               # {'USD': 1.0914, 'GBP': 0.84058}

param_url = base_url + '?symbols=GBP' + '&' + 'base=USD'
data = requests.get(param_url).json()

usd_to_gbp = data['rates']['GBP']   # 0.7701850834
```

### Obtendo Taxas de Câmbio Históricas

Pegar uma taxa de câmbio de uma data específica.

```python
# A data específica é 2016-01-06
historical_url = base_url + '/2016-01-26'
response = requests.get(historical_url)
data = response.json()
print(json.dumps(data, indent=4))       # Aqui o resultado esperado
```

### Extraindo dado de um Período de Tempo

```python
time_period = base_url + '/history' + '?start_at=2017-04-26&end_at=2018-04-26' + '&symbols=GBP'

# Traduções da linha de cima:
# time_period = periodo_de_tempo
# start_at = comeca_em
# end_at = termina_em

data = requests.get(time_period).json()
print(json.dumps(data, indent=4), sort_keys=True)
# O parâmetro sort_keys=True deixa tudo em ordem cronológica
```

### Exemplo Completo

No exemplo abaixo, uma construção de um conversor de taxas.

```python
import requests

def cover():
    msg = ' BEM-VINDO AO CONVERSOR DE TAXAS DE CÂMBIO '
    print('\033[1;32;42m=\033[0m'*70)
    print(msg.center(70, '~'))
    print('\033[1;32;42m=\033[0m'*70)

cover()
print('Você gostaria de converter dinheiro de uma data específica?')
date = input('Por favor, digite a data (yyy-mm-dd) ou digite "latest" para a data mais recente: ')
base = input('Converter de (moeda): ')
curr = input('Converter para (meoda): ')
quan = float(input('Quanto você gostaria de converter? Digite aqui: '.format(base)))

base_url = 'https://api.exchangeratesapi.io/latest'
url = base_url + '/' + date + '?base=' + base + '&symbols=' + curr
response = requests.get(url)

if response.ok is False:
    print(f'Error: {response.status_code}:')
    print(response.json()['error'])
else:
    data = response.json()
    rate = data['rates'][curr]
    result = quan * rate
    print('{1} is equal to {2} {3}, based upon\n'
          'exchange rates on {4}'.format(quan, base, result, curr, data['date']))
```
