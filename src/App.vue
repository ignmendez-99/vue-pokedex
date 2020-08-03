<template>
  <v-app>
    <v-main>
      <router-view />   <!-- esto esta invocando a Home.vue -->
    </v-main>
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
