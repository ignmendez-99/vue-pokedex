<template>
  <v-container fluid class="py-0">
    <v-row>
      <v-col cols="3">
        <PokemonList />
      </v-col>
      <v-col>
        <PokemonDisplay />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
// importamos los Components de la lista y el display
import PokemonList from "../components/PokemonList.vue";
import PokemonDisplay from "../components/PokemonDisplay.vue";

export default {
  name: "Home",
  components: {
    PokemonList,
    PokemonDisplay
  }
};
</script>
