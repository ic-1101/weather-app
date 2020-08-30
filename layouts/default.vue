<template>
  <v-app dark>
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-toolbar-title class="d-flex flex-row-reverse">
        <v-bottom-sheet v-model="sheet" persistent light>
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              color="green"
              dark
              v-bind="attrs"
              v-on="on"
            >
              Follow Me
            </v-btn>
          </template>
          <v-sheet class="text-center" height="200px">
            <v-btn
              class="mt-6"
              text
              color="error"
              @click="sheet = !sheet"
            >close</v-btn>
            <div class="py-3">Follow me for more 'awesome' work.</div>
            <v-btn icon href="https://github.com/ic-1101" target="_blank">
              <v-icon large color="purple darken-2">mdi-github</v-icon>
            </v-btn>
            <v-btn icon href="https://gitlab.com/nk.t" target="_blank">
              <v-icon large color="yellow darken-2">mdi-gitlab</v-icon>
            </v-btn>
            <v-btn icon href="https://www.instagram.com/nk_tuhin/" target="_blank">
              <v-icon large color="red darken-2">mdi-instagram</v-icon>
            </v-btn>
            <v-btn icon href="https://bitbucket.org/nk.t" target="_blank">
              <v-icon large color="blue darken-2">mdi-bitbucket</v-icon>
            </v-btn>
            <v-btn icon href="https://www.linkedin.com/in/nktuhin/" target="_blank">
              <v-icon large color="blue darken-2">mdi-linkedin</v-icon>
            </v-btn>
          </v-sheet>
        </v-bottom-sheet>
      </v-toolbar-title>
    </v-app-bar>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/',
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Weather App',
      sheet: false,
    }
  },
}
</script>
