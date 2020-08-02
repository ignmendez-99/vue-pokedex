<template>
  <v-container fluid>
    <v-row>
      <v-card>
        <v-card-title>Pokémon List</v-card-title>
        <v-row justify="center" align="center" v-if="this.loadingPokemons">
          <div class="ma-12">
            <v-progress-circular
              class="loading-circle"
              indeterminate
              color="red"
            ></v-progress-circular>
          </div>
        </v-row>
        <v-list v-else avatar rounded>
          <v-list-item-group v-model="pokemons.length" color="primary">
            <v-list-item v-for="(pokemon, i) in pokemons" :key="i">
              <!-- <v-list-item-avatar>
                <v-img :src="pokemon.image"></v-img>
              </v-list-item-avatar> -->
              <v-list-item-content>
                <v-list-item-title v-html="pokemon.name"></v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-card>
      <v-btn @click="getAllPokemons">HOLA</v-btn>
    </v-row>
  </v-container>
</template>

<script>
const pokedex = require("pokeapi-js-wrapper");
const P = new pokedex.Pokedex();
export default {
  name: "PokemonList",
  data: () => {
    return {
      pokemons: [],
      loadingPokemons: true
    };
  },
  methods: {
    getAllPokemons() {
      let pokemonsCopy = this.pokemons;
      let thisVue = this;
      P.getPokemonsList().then(function(response) {
        response.results.forEach(pokemon => {
          pokemonsCopy.push({
            name: pokemon.name,
            url: pokemon.url,
            photo: {}
          });
          thisVue.getPokemonPhoto(pokemon.url);
        });
        thisVue.loadingPokemons = false;
      });
    },
    getPokemonPhoto(url) {
      console.log(url);
    }
  }
};
</script>
