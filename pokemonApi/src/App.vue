<script>
import axios from "axios";
import Pokemons from "./components/Pokemons.vue";

export default {
  components: {
    Pokemons,
  },
  data() {
    return {
      pokemons: [],
      count: 0,
    };
  },
  mounted() {
    this.traerPokemons();
  },
  methods: {
    async traerPokemons() {
      try {
        const url = "https://pokeapi.co/api/v2/pokemon";
        const { data } = await axios.get(url);
        data.results.forEach(async (pokemon) => {
          const { data: pokemonData } = await axios.get(pokemon.url);
          this.pokemons.push({
            name: pokemonData.name,
            img: pokemonData.sprites.other.dream_world.front_default,
          });
        });
      } catch (error) {
        console.error(error);
      }
    },
    clickPokemon(index) {
      const pokemon = this.pokemons[index];
      if (this.pokemonExiste(pokemon)) {
        if (!pokemon.nitido) {
          this.count++;
          pokemon.nitido = true;
        }
      } else {
        alert("¡Ese pokémon no existe!");
      }
    },
  },
  computed: {
    pokemonExiste() {
      return (pokemon) =>
        pokemon.name.toLowerCase() === pokemon.input.toLowerCase().trim();
    },
  },
};
</script>

<template>
  <div id="App">
    <div class="contador">
      <img class="logo" src="./assets/img/logopoke.png" alt="Logo Pokémon" />
      <h1>¿Quién es ese Pokémon?</h1>
      <p>Pokémones descubiertos: {{ count }}</p>
    </div>

    <div class="pokemones">
      <Pokemons
        v-for="(pokemon, i) in pokemons"
        :pokemon="pokemon"
        :index="i"
        @pokemonEncontrado="clickPokemon"
      />
    </div>
  </div>
</template>

<style scoped>
.contador {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.logo {
  width: 25%;
}

.pokemones {
  display: grid;
  grid-template-columns: repeat(4, 150px);
  justify-content: center;
  gap: 80px;
}

p {
  font-family: "VT323", serif;
  font-size: 25px;
}

h1 {
  font-family: "Chivo", sans-serif;
  font-weight: 300;
}
</style>
