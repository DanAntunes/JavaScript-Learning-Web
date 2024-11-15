# JavaScript-Learning-Web Aprendizado em Java Script pela DIO

## Usando o Fetch API

O *fetch()* é uma função nativa do JavaScript para realizar requisições HTTP. É amplamente utilizada por ser fácil de usar e integrada aos navegadores modernos.

**Exemplo Básico:** Requisição **GET**
Vamos buscar informações sobre o Pokémon Pikachu na PokeAPI:

```JavaScript
fetch("https://pokeapi.co/api/v2/pokemon/pikachu")
  .then(response => {
    if (!response.ok) {
      throw new Error("Erro na requisição: " + response.status);
    }
    return response.json(); // Converte a resposta para JSON
  })
  .then(data => {
    console.log("Dados do Pikachu:", data); // Manipula os dados retornados
  })
  .catch(error => {
    console.error("Erro ao buscar dados:", error); // Lida com erros
  });
```
Explicação:

**fetch(url):** Inicia uma requisição para a URL especificada.

**response.ok:** Verifica se a resposta foi bem-sucedida (status HTTP 200–299).

**response.json():** Converte a resposta do servidor para o formato JSON.

**.then():** Manipula os dados da resposta.

**.catch():** Captura e trata erros.

Exemplo com Parâmetros na URL <mark>(Query String)</mark> Para buscar 10 Pokémon a partir do ID 20:

```JavaScript
fetch("https://pokeapi.co/api/v2/pokemon?limit=10&offset=20")
  .then(response => response.json())
  .then(data => {
    console.log("Lista de Pokémon:", data.results); // Exibe os resultados
  })

  .catch(error => console.error("Erro:", error));
```

Exemplo Avançado: Requisição **POST**

No caso de APIs que aceitam dados via POST (não é o caso da PokeAPI, mas útil em APIs que permitem criar recursos):

```JavaScript
fetch("https://api.exemplo.com/create", {
  method: "POST", // Define o método HTTP
  headers: {
    "Content-Type": "application/json" // Define o tipo de conteúdo
  },
  body: JSON.stringify({
    name: "Bulbasaur",
    type: "Grass"
  }) // Envia os dados no corpo da requisição
})
  .then(response => response.json())
  .then(data => {
    console.log("Recurso criado:", data);
  })
  .catch(error => console.error("Erro ao criar recurso:", error));
  ```

### Exemplo com Manipulação Complexa

Agora, vamos buscar uma lista de Pokémon e trabalhar com os dados retornados, como exibir nomes e habilidades dos primeiros 5 Pokémon.

```JavaScript
  fetch("https://pokeapi.co/api/v2/pokemon?limit=5")
  .then(response => {
    if (!response.ok) {
      throw new Error("Erro na requisição: " + response.status);
    }
    return response.json();
  })
  .then(data => {
    // Itera sobre os resultados e faz uma nova requisição para cada Pokémon
    const promises = data.results.map(pokemon =>
      fetch(pokemon.url).then(res => res.json())
    );

    // Aguarda todas as requisições serem resolvidas
    return Promise.all(promises);
  })
  .then(pokemonData => {
    // Manipula os dados de cada Pokémon
    pokemonData.forEach(pokemon => {
      console.log(`Nome: ${pokemon.name}`);
      console.log(`Habilidades: ${pokemon.abilities.map(a => a.ability.name).join(", ")}`);
    });
  })
  .catch(error => {
    console.error("Erro ao buscar Pokémon:", error);
  });
```

**Explicação:**

**data.results.map():** Para cada Pokémon na lista, faz uma nova requisição à URL específica.

**Promise.all(promises):** Aguarda todas as requisições serem resolvidas antes de prosseguir.

**Manipulação dos Dados:** Itera sobre os resultados de cada requisição para exibir informações detalhadas.


## Explicação do Código
[poke-api.js](./src/assets/scripts/poke-api.js)

1. **Função** - **convertPokeApiDetailToPokemon**

Essa função recebe o detalhe de um Pokémon retornado pela API e o converte em uma instância personalizada de um objeto **Pokemon**. Ela:

 - Cria um novo objeto Pokemon.

 - Atribui propriedades como **number**, **name**, **types**, **type** e **photo** com base nos dados recebidos.

  - Retorna esse objeto **Pokemon**.

  ----

2. **Método** - **pokeApi.getPokemonDetail**

Este método é responsável por:

 - Receber um Pokémon da lista retornada pela API (que contém apenas nome e URL do detalhe).

 - Fazer uma requisição para a URL do detalhe (**pokemon.url**).

 - Converter a resposta detalhada em um objeto Pokemon usando **convertPokeApiDetailToPokemon**.


---

3. **Método** - **pokeApi.getPokemons**

Este método:

Gera a URL da requisição para buscar uma lista de Pokémon com base em um offset e limit.

 - Faz a requisição à API para obter a lista de Pokémon.

 - Extrai os resultados da lista (cada item contém apenas o nome e a URL para o detalhe).

 - Mapeia cada Pokémon da lista para uma chamada a **pokeApi.getPokemonDetail**.

 - Usa **Promise.all** para aguardar todas as requisições de detalhes serem resolvidas.

 - Retorna os detalhes de todos os Pokémon como uma lista de objetos **Pokemon**.

---

### Passo a Passo do Fluxo de Execução

Se você executar **pokeApi.getPokemons(0, 5)**, veja o que acontece:

1. Requisição Inicial: A função cria a URL:

```bash
https://pokeapi.co/api/v2/pokemon?offset=0&limit=5
```
 Faz a requisição e recebe um JSON com uma lista de Pokémon:

```json
{
    "results": [
        { "name": "bulbasaur", "url": "https://pokeapi.co/api/v2/pokemon/1/" },
        { "name": "ivysaur", "url": "https://pokeapi.co/api/v2/pokemon/2/" },
        ...
    ]
}
```
2. Detalhes de Cada Pokémon: Para cada item em **results**, ele:

 - Faz uma requisição para a URL de detalhe do Pokémon.

 - Exemplo para bulbasaur:

```bash
https://pokeapi.co/api/v2/pokemon/1/
```

 - Obtém detalhes como **id**, **name**, **types**, **sprites**, etc.

3. Conversão para Objeto **Pokemon**: Cada detalhe retornado é convertido em um objeto Pokemon personalizado:

```JavaScript
{
    number: 1,
    name: "bulbasaur",
    types: ["grass", "poison"],
    type: "grass",
    photo: "https://raw.githubusercontent.com/.../dream_world/front_default.png"
}
```

4. Retorno da Lista Completa: Após processar todos os detalhes, a função retorna uma lista com os objetos **Pokemon** personalizados.