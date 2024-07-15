<template>
  <v-app>
    <v-main>
      <ConfigurationPanel/>
    </v-main>
  </v-app>
</template>

<script>
import ConfigurationPanel from './components/ConfigurationPanel.vue';

export default {
  name: 'App',

  components: {
    ConfigurationPanel,
  },

  data: () => ({
    //
  }),
}
</script>
