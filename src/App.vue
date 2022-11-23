<template>
  <v-app>
    <v-app-bar
      app
      color="Black"
      dark
    >
      <div class="d-flex align-center">
        <h1>RDG Maker</h1>
      </div>

      <v-spacer></v-spacer>

      <h3>By EinUnknownCoder</h3>

      <template v-slot:extension>
        <v-tabs align-with-title grow>
          <v-tab @click="tab = 0">Hello World</v-tab>
          <v-tab @click="tab = 1">Create Playlist</v-tab>
          <v-tab @click="tab = 2">Edit Database</v-tab>
        </v-tabs>
      </template>
    </v-app-bar>

    <v-main>
      <HelloWorld v-if="tab == 0"/>
      <CreatePlaylist v-if="tab == 1" />
      <EditDatabase v-if="tab == 2" />
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';
import CreatePlaylist from './components/CreatePlaylist';
import EditDatabase from './components/EditDatabase';

export default {
  name: 'App',

  components: {
    HelloWorld,
    CreatePlaylist,
    EditDatabase
  },

  data: () => ({
    tab: 0,
  })
};
</script>
