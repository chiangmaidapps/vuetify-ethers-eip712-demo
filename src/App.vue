<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-toolbar-title class="text-uppercase white--text">
          <span class="font-weight-light">Vuetify & Ethers eip712 demo</span>
        </v-toolbar-title>
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://docs.ethers.io/v5/single-page/#/v5/"
        target="_blank"
        text
      >
        <span class="mr-2">Ethers Docs</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>

      <v-btn
        href="https://eips.ethereum.org/EIPS/eip-712"
        target="_blank"
        text
      >
        <span class="mr-2">EIP 712</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>

      <v-btn
        href="https://vuetifyjs.com/en/components/alerts/#usage"
        target="_blank"
        text
      >
        <span class="mr-2">UI Docs</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <HelloWorld/>
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';

export default {
  name: 'App',
  components: {
    HelloWorld,
  }
};
</script>
