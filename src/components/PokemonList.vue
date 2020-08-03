<script>/* eslint-disable prettier/prettier */</script>

<template>
  <v-container fluid>
    <v-row>
      <v-card>
        <v-card-title>Pokémon List</v-card-title>
        <v-row
          justify="center"
          align="center"
          v-if="this.loadingInitialPokemons"
        >
          <div class="ma-12">
            <v-progress-circular
              indeterminate
              color="red"
            ></v-progress-circular>
          </div>
        </v-row>
        <v-list v-else avatar rounded width="270">
          <v-list-item-group v-model="displayedPokemons.length" color="primary">
            <v-list-item v-for="(pokemon, i) in displayedPokemons" :key="i">
              <v-list-item-avatar>
                <v-img :src="pokemon.imageURL"></v-img>
              </v-list-item-avatar>
              <v-list-item-content>
                <v-list-item-title v-html="pokemon.name"></v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-item-group>
        </v-list>
        <v-row justify="center" align="center">
          <v-btn
            class="mb-5"
            :loading="this.loadingMorePokemons"
            v-show="!this.loadingInitialPokemons"
            @click="loadMorePokemons()"
          >SHOW MORE</v-btn>
        </v-row>
      </v-card>
    </v-row>
  </v-container>
</template>

<script>
const pokedex = require("pokeapi-js-wrapper");
const P = new pokedex.Pokedex();
const axios = require("axios");
export default {
  name: "PokemonList",
  data: () => {
    return {
      initialLoadingPokemons: 50,
      numberOfDisplayedPokemons: 50,
      displayedPokemons: [],
      pokemonsNotSorted: [],
      loadingInitialPokemons: true,
      loadingMorePokemons: false,

      // huge data
      allPokemonsDataURL: []
    };
  },
  methods: {
    loadMorePokemons() {
      this.loadingMorePokemons = true;
      const currentMax = this.numberOfDisplayedPokemons;
      for (let index = currentMax; index < currentMax + 50; index++) {
        axios.get(this.allPokemonsDataURL[index]).then(response => {
          this.pokemonsNotSorted.push({
            id: response.data.id,
            name: response.data.name,
            imageURL: response.data.sprites.front_default
          });
        });   
      }
      this.numberOfDisplayedPokemons = currentMax + 50;
      setTimeout(() => {
        this.pokemonsNotSorted.sort((pokemon1, pokemon2) => {
          return pokemon1.id - pokemon2.id;
        });
        this.displayedPokemons = this.pokemonsNotSorted.slice();
        this.loadingMorePokemons = false;
      }, 3000);
    }
  },
  mounted() {
    let thisVue = this;
    let counter = 0;
    P.getPokemonsList().then(response => {
      response.results.forEach(pokemon => {
        const pokemonDataURL = pokemon.url;
        thisVue.allPokemonsDataURL.push(pokemonDataURL);
        if (counter < thisVue.initialLoadingPokemons) {
          axios.get(pokemonDataURL).then(response => {
            thisVue.pokemonsNotSorted.push({
              id: response.data.id,
              name: response.data.name,
              imageURL: response.data.sprites.front_default
            });
          });
          counter++;
        }
      });
      setTimeout(() => {
        thisVue.pokemonsNotSorted.sort((pokemon1, pokemon2) => {
          return pokemon1.id - pokemon2.id;
        });
        thisVue.displayedPokemons = thisVue.pokemonsNotSorted.slice();
        thisVue.loadingInitialPokemons = false;
      }, 3000);
    });
  }
};
</script>
