# Desenvolvendo-sua-Pokédex-com-Vue.js

Desenvolver uma Pokédex usando Vue.js é uma excelente oportunidade para explorar um projeto modular. Vamos estruturar o projeto e fornecer exemplos de código para cada parte essencial.

### Estrutura do Projeto

1. **Componentes Vue**
   - **App.vue**: Componente principal que renderiza a aplicação.
   - **PokemonList.vue**: Componente para exibir a lista de Pokémon.
   - **PokemonCard.vue**: Componente para exibir detalhes de um Pokémon específico.
   - **PokemonFilter.vue**: Componente para filtrar Pokémon por tipo ou outra categoria.

2. **Serviços e Dados**
   - **pokemonService.js**: Um serviço para obter dados de Pokémon usando a PokéAPI ou outro serviço de sua escolha.
   - **pokemonData.js**: Dados estáticos ou mockados dos Pokémon para desenvolvimento local.

### Exemplo de Código

#### App.vue

```vue
<template>
  <div id="app">
    <header>
      <h1>Pokédex</h1>
    </header>
    <main>
      <PokemonList />
    </main>
    <footer>
      Desenvolvido por [seu nome] &copy; 2024
    </footer>
  </div>
</template>

<script>
import PokemonList from './components/PokemonList.vue';

export default {
  name: 'App',
  components: {
    PokemonList,
  }
}
</script>

<style>
/* Estilos globais */
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Arial', sans-serif;
}
header, footer {
  text-align: center;
  padding: 10px;
  background-color: #f0f0f0;
}
main {
  padding: 20px;
}
</style>
```

#### PokemonList.vue

```vue
<template>
  <div class="pokemon-list">
    <PokemonCard
      v-for="pokemon in filteredPokemon"
      :key="pokemon.id"
      :pokemon="pokemon"
    />
  </div>
</template>

<script>
import PokemonCard from './PokemonCard.vue';
import { pokemonService } from '../services/pokemonService';

export default {
  name: 'PokemonList',
  components: {
    PokemonCard,
  },
  data() {
    return {
      pokemonList: [],
      filter: '',
    };
  },
  computed: {
    filteredPokemon() {
      if (this.filter) {
        return this.pokemonList.filter(pokemon =>
          pokemon.name.toLowerCase().includes(this.filter.toLowerCase())
        );
      } else {
        return this.pokemonList;
      }
    }
  },
  async created() {
    this.pokemonList = await pokemonService.getAllPokemon();
  },
};
</script>

<style scoped>
/* Estilos locais */
.pokemon-list {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}
</style>
```

#### PokemonCard.vue

```vue
<template>
  <div class="pokemon-card">
    <img :src="pokemon.imageUrl" :alt="pokemon.name" />
    <h2>{{ pokemon.name }}</h2>
    <p>{{ pokemon.description }}</p>
  </div>
</template>

<script>
export default {
  name: 'PokemonCard',
  props: {
    pokemon: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
/* Estilos locais */
.pokemon-card {
  border: 1px solid #ccc;
  padding: 10px;
  text-align: center;
  width: 200px;
  background-color: #f9f9f9;
}
.pokemon-card img {
  max-width: 100%;
  height: auto;
}
</style>
```

#### pokemonService.js (Exemplo básico)

```javascript
const pokemonService = {
  async getAllPokemon() {
    try {
      const response = await fetch('https://pokeapi.co/api/v2/pokemon');
      if (!response.ok) {
        throw new Error('Falha ao carregar dados');
      }
      const data = await response.json();
      return data.results.map((pokemon, index) => ({
        id: index + 1,
        name: pokemon.name,
        imageUrl: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${index + 1}.png`,
        description: `Descrição do Pokémon ${pokemon.name}`,
      }));
    } catch (error) {
      console.error('Erro ao buscar Pokémon:', error);
      return [];
    }
  },
};

export { pokemonService };
```

### Considerações Finais

Este esboço de projeto utiliza Vue.js para criar uma aplicação de Pokédex simples. Cada componente é modular e alinhado à esquerda, seguindo as melhores práticas de desenvolvimento Vue. Lembre-se de ajustar e expandir conforme necessário, especialmente ao integrar com APIs externas como a PokéAPI para obter dados reais de Pokémon.
