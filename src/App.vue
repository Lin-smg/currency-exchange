<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >

    <h1>Currency Exchange</h1>
    
    </v-app-bar><br>

    <v-main>
      <currency></currency>
    </v-main>
  </v-app>
</template>

<script>
import Currency from '@/components/Currency'

export default {
  name: 'App',

  components: {
    Currency
  },

  data: () => ({
    //
  }),
};
</script>
