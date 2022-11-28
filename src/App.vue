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
          <v-tab @click="tab = 0">Create Playlist from Excel</v-tab>
          <v-tab @click="tab = 1">Create VAO Playlist</v-tab>
          <v-tab @click="tab = 2">Edit VAO Database</v-tab>
        </v-tabs>
      </template>
    </v-app-bar>

    <v-main>
      <CreateExcelPlaylist v-if="tab == 0"/>
      <CreateVAOPlaylist v-if="tab == 1" />
      <EditDatabase v-if="tab == 2" />
    </v-main>
  </v-app>
</template>

<script>
import CreateExcelPlaylist from './components/CreateExcelPlaylist';
import CreateVAOPlaylist from './components/CreateVAOPlaylist';
import EditDatabase from './components/EditDatabase';

export default {
  name: 'App',

  components: {
    CreateExcelPlaylist,
    CreateVAOPlaylist,
    EditDatabase
  },

  data: () => ({
    tab: 0,
  })
};
</script>
