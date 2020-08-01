<template>
  <div class="home">
    <PokemonList/>
    <PokemonDisplay/>
  </div>
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
