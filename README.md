# JavaScript-Learning-Web Aprendizado em Java Script pela DIO

## API

APIs (Application Programming Interfaces) são ferramentas que permitem que diferentes softwares se comuniquem entre si. Elas fornecem uma maneira estruturada de solicitar e receber dados de um servidor, geralmente através de requisições HTTP, como **GET**, **POST**, **PUT** e **DELETE**. Quando você utiliza uma API, está acessando informações de um servidor remoto para utilizar ou manipular esses dados no seu próprio sistema.

## URL PATH

O URL Path (ou Caminho da URL) é a parte de uma URL que vem após o domínio e ajuda a indicar a localização específica de uma página ou recurso em um site. Em uma URL típica, o caminho é o trecho que informa ao servidor o que o usuário deseja acessar. Ele é essencial na navegação de sites e no uso de APIs, pois indica ao servidor onde encontrar o conteúdo solicitado.

```JavaScript
https://dominio.com/path/do/recurso
```

### Aqui está uma divisão dos componentes principais:

 - Protocolo: **https://** - define o protocolo de acesso à web, como HTTP ou HTTPS.
 - Domínio: **dominio.com** - indica o nome do site ou servidor.
 - Path: **/path/do/recurso** - o caminho específico do recurso no servidor.

### Como Funciona o URL Path

O URL Path age como um “endereço” dentro de um site ou sistema, informando ao servidor qual conteúdo carregar. No caso de uma aplicação de programação, o caminho define que tipo de recurso ou dado a aplicação deseja acessar. Ele pode incluir subpastas ou níveis, o que permite estruturar conteúdos de forma hierárquica.

Exemplos:

 - **https://exemplo.com/blog** - o servidor entende que deve carregar a página “blog”.

 - **https://exemplo.com/produtos/camisa** - direciona para uma página específica, como a de produtos relacionados a camisas.

 - **https://api.exemplo.com/v1/users/123** - em uma API, o path direciona para informações do usuário com ID 123.

### Path em APIs: Exemplo com PokeAPI

Em APIs, o URL Path é fundamental para identificar o tipo de dado a ser acessado. A **PokeAPI**, por exemplo, organiza os dados de forma que cada path corresponda a um recurso:

 - **https://pokeapi.co/api/v2/pokemon/pikachu**: Retorna os dados do Pokémon Pikachu.

 - **https://pokeapi.co/api/v2/type/3**: Retorna informações sobre o tipo com ID 3 (neste caso, provavelmente “Flying”).

 - **https://pokeapi.co/api/v2/ability/65**: Fornece informações sobre uma habilidade específica com ID 65.

        A divisão em paths permite que a PokeAPI organize os dados e possibilita ao cliente (aplicação ou usuário) especificar o recurso exato a ser acessado.

### URL Path e Parâmetros

Os paths também podem ser combinados com parâmetros de URL para uma busca mais específica:

 - /pokemon?**limit=10**: A API pode usar limit para retornar apenas os primeiros 10 Pokémon.

 - /pokemon?**type=fire**: Pode retornar uma lista apenas com Pokémon do tipo Fogo.

 ## QUERY STRING

 A query string é uma parte da URL usada para enviar parâmetros adicionais em uma requisição. Ela permite que os dados sejam passados ao servidor sem afetar o URL principal e é comumente usada para filtros de busca, paginação, e configuração de exibição de dados em páginas ou APIs.

### Estrutura de uma Query String

A query string é sempre adicionada após o URL principal, precedida por um ponto de interrogação **(?)**. Os parâmetros individuais na query string são separados por **&** e têm o formato **chave=valor**.

**Exemplo de URL com Query String**

Imagine que você deseja fazer uma busca na PokeAPI para listar 10 Pokémon do tipo **"fire"**. A URL com query string ficaria assim:

```bash
https://pokeapi.co/api/v2/pokemon?type=fire&limit=10
```
**Neste caso:**

 - **?** inicia a query string.

 - **type=fire** é o primeiro parâmetro, que especifica o tipo de Pokémon.

 - **&** separa múltiplos parâmetros.

 - **limit=10** define o limite de resultados.

### Como a Query String é Usada

A query string permite enviar informações de forma dinâmica sem alterar a estrutura básica da URL. Aqui estão alguns exemplos comuns de uso:

**Filtro de Dados**

 - Em uma API, podemos filtrar os resultados de acordo com critérios específicos. Por exemplo, **?type=fire** filtra apenas Pokémon do tipo **"fire"**.

**Paginação**

 - Em sistemas que retornam grandes conjuntos de dados, a query string pode ser usada para controlar a página e a quantidade de resultados. **Exemplo:**

```bash
https://pokeapi.co/api/v2/pokemon?limit=20&offset=40
```

 - Aqui, **limit=20** significa que queremos 20 resultados por página, e **offset=40** significa que queremos começar a partir do 41º registro.

**Ordenação**

 - Em alguns sistemas, podemos ordenar os dados usando parâmetros de query string, como **?sort=name** ou **?order=asc**.

**Busca**

 - A query string também permite buscar por termos específicos, como em **?search=pikachu**.
 
### Manipulando Query String em JavaScript

Em JavaScript, você pode adicionar ou manipular query strings ao fazer requisições. Veja como extrair parâmetros de uma query string de forma prática:

```JavaScript
const url = new URL("https://pokeapi.co/api/v2/pokemon?type=fire&limit=10");
const params = new URLSearchParams(url.search);

console.log(params.get("type")); // "fire"
console.log(params.get("limit")); // "10"
```

Ou ao fazer uma requisição com fetch:

```JavaScript
fetch("https://pokeapi.co/api/v2/pokemon?limit=10&type=fire")
  .then(response => response.json())
  .then(data => {
    console.log(data);
  });
```

## Diferença entre URL Path e Query String

 1. **URL Path:** É a parte da URL que representa a estrutura de diretórios e identifica o recurso específico que estamos acessando no servidor. O path vem logo após o domínio e é separado por barras **(/)**, indicando hierarquias.

**Exemplo**: 
```bash
https://pokeapi.co/api/v2/pokemon/25
```
 - Aqui, o path **/api/v2/pokemon/25** indica que queremos o Pokémon com ID 25.

2. **Query String:** É uma parte opcional da URL que permite enviar informações adicionais ou parâmetros de configuração para o servidor. Ela é iniciada com um ponto de interrogação **(?)** e contém pares **chave=valor** separados por **&**.

**Exemplo:** 
```bash
https://pokeapi.co/api/v2/pokemon?limit=10&offset=20
```

 - Neste caso, a query string **?limit=10&offset=20** está adicionando um limite de 10 resultados e começando a partir do vigésimo registro, sem mudar o path principal **(/api/v2/pokemon)**.

### Parâmetros no URL Path vs. Parâmetros na Query String

 - **Parâmetros no Path:** Geralmente são parte fixa da estrutura do URL e representam recursos específicos. Um path com parâmetros ajuda a navegar diretamente para um recurso único. Em RESTful APIs, como na PokeAPI, o path usa parâmetros como IDs ou nomes de forma direta:

**Exemplo:** 
```bash
https://pokeapi.co/api/v2/pokemon/pikachu
– acessa o Pokémon chamado Pikachu.
``` 

 - **Parâmetros na Query String**: São usados para adicionar filtros, opções de configuração e não são obrigatórios para acessar o recurso principal. Eles alteram a resposta sem alterar o recurso que está sendo acessado. Em APIs, query strings são frequentemente usadas para:

    - **Filtrar dados**: **?type=fire** – retorna Pokémon do tipo fogo.
   - **Paginar resultados**: **?limit=10&offset=20** – controla quantos registros são retornados e a partir de onde. 
   - **Ordenar dados** ou definir outros critérios.

Exemplo de Uso com a PokeAPI

<mark>Combinando os dois:<mark>

1. Acessar um recurso específico via URL Path:

**https://pokeapi.co/api/v2/pokemon/25** – O path indica que queremos o Pokémon com ID 25.

2. Configurar resultados via Query String:

**https://pokeapi.co/api/v2/pokemon?limit=10&offset=20** – O path **/pokemon** indica o recurso Pokémon, enquanto a query string limita os resultados a 10, começando do registro 20.

 ## REQUEST METHODS

 Os Request Methods (ou Métodos de Requisição) são comandos utilizados em protocolos de comunicação na web, como o HTTP, para definir qual ação queremos que o servidor realize com um determinado recurso. Cada método representa uma ação específica e é usado em aplicações web e APIs para obter, enviar, modificar ou excluir dados. Os métodos de requisição mais comuns incluem **GET**, **POST**, **PUT**, **PATCH** e **DELETE**. 

 ### Principais Métodos de Requisição

    1 - GET

    Objetivo: Solicitar dados de um servidor sem modificar nada.

    Uso Comum: Para obter informações. Por exemplo, em uma API de Pokémon (como a PokeAPI), você pode usar GET para buscar informações sobre um Pokémon específico.

**Exemplo:**
```JavaScript
GET /api/v2/pokemon/pikachu
```
**Resultado:** O servidor retorna os dados do Pokémon Pikachu.

    2 - POST

    Objetivo: Enviar dados para o servidor, geralmente para criar um novo recurso.

    Uso Comum: Em APIs, o POST é frequentemente utilizado para criar novos registros, como um novo usuário, uma nova postagem, etc.

**Exemplo:**
```JavaScript
POST /api/v2/pokemon
```

**Corpo da requisição:**

```json
{
  "name": "bulbasaur",
  "type": "grass"
}
```

**Resultado:** O servidor cria um novo recurso com os dados fornecidos.


    3 - PUT

    Objetivo: Atualizar completamente um recurso existente no servidor.

    Uso Comum: Usado quando queremos substituir um recurso por uma nova versão. Todos os dados antigos do recurso são substituídos pelos dados novos.

**Exemplo:**

```JavaScript
PUT /api/v2/pokemon/1
```
**Corpo da requisição:**
    
```json
{
  "name": "bulbasaur",
  "type": "poison"
}
```
**Resultado:** O recurso de ID 1 (Bulbasaur) é atualizado para ter o tipo “poison”.

    4 - PATCH

    Objetivo: Modificar parcialmente um recurso existente.

    Uso Comum: Atualizar apenas alguns atributos de um recurso, sem substituir o recurso inteiro.

**Exemplo:**

```JavaScript
PATCH /api/v2/pokemon/1
```
    
**Corpo da requisição:**
```json
{
  "type": "grass"
}
```
**Resultado:** Apenas o atributo type do Pokémon com ID 1 é atualizado para “grass”.

    5 - DELETE

    Objetivo: Remover um recurso do servidor.

    Uso Comum: Excluir registros, como deletar um usuário ou remover uma postagem.

**Exemplo:**
```JavaScript
DELETE /api/v2/pokemon/1
```
**Resultado:** O servidor remove o Pokémon com ID 1.