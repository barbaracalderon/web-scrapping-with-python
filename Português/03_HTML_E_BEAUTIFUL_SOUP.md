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

_Continua_...