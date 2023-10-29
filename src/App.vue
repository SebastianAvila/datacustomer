<template>
  <v-app>
    <v-app-bar app color="light-blue darken-4" dark>
      <div class="d-flex align-center">
        <h1>Obtencion de datos</h1>
      </div>

      <v-spacer></v-spacer>

      <v-img alt="Vuetify Logo" class="shrink mr-2" contain
        src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png" transition="scale-transition" width="40" />

      <v-img alt="Vuetify Name" class="shrink mt-1 hidden-sm-and-down" contain min-width="100"
        src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png" width="100" />
    </v-app-bar>

    <v-main>
      <router-view />
    </v-main>


    <v-footer dark padless>
      <v-card class="flex" flat tile>
        <v-card-title>
          <p class="subheading">Desarrollado con: <strong>Vue</strong> y <strong>Vuetify</strong> por <strong>Sebastian
              Avila</strong> </p>

          <v-spacer></v-spacer>

          <v-btn v-for="icon in icons" :key="icon" class="mx-4" dark icon>
            <v-icon size="24px">
              {{ icon }}
            </v-icon>
          </v-btn>
        </v-card-title>

        <v-card-text class="py-2 white--text text-center">
          {{ new Date().getFullYear() }}
        </v-card-text>
      </v-card>
    </v-footer>
  </v-app>
</template>

<script>

export default {
  data() {
    return {
      icons: [
        'mdi-facebook',
        'mdi-twitter',
        'mdi-linkedin',
        'mdi-instagram',
        'mdi mdi-github',
      ],
    }
  }
}
</script>
