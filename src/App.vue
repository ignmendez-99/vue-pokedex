<template>
  <v-app>
    <v-content>
      <router-view />   <!-- esto esta invocando a Home.vue -->
    </v-content>
  </v-app>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    //
  })
};
</script>
