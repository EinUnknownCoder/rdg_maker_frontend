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

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Latest Release</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <v-simple-table>
        <thead>
          <tr>
            <th>
              Title
            </th>
            <th>
              Artist
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="entry in info" :key="entry.URL">
            <td>{{entry.Title}}</td>
            <td>{{entry.Artist}}</td>
          </tr>
        </tbody>
      </v-simple-table>
      <HelloWorld/>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios';

import HelloWorld from './components/HelloWorld';

export default {
  name: 'App',

  components: {
    HelloWorld,
  },

  data: () => ({
    info: null
  }),

  mounted() {
    axios
    .get('http://127.0.0.1:8000/all')
    .then(response => (this.info = response.data))
  }
};
</script>
