<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="6">
        <v-btn dark block @click="showExcel">Find/Show Excel</v-btn>
      </v-col>
      <v-col cols="12" md="6">
        <v-btn color="primary" block>Create Playlist</v-btn>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <v-simple-table>
          <thead>
            <tr>
              <th>YouTube-URL</th>
              <th>Artist</th>
              <th>Title</th>
              <th>Description</th>
              <th>Start</th>
              <th>End</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="song in songs" :key="song.URL">
              <td> <a :href="song.URL + '?t=' + song.Start" target="_blank">{{ song.URL }}</a></td>
              <td> {{ song.Artist }} </td>
              <td> {{ song.Title }} </td>
              <td> {{ song.Description }} </td>
              <td> {{ song.Start }} </td>
              <td> {{ song.End }} </td>
            </tr>
          </tbody>
        </v-simple-table>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: 'CreateExcelPlaylist',

  data: () => ({
    songs: []
  }),

  computed: {
    fileProperties() {
      return Object.getOwnPropertyNames(this.file[0]['values'])
    }
  },

  methods: {
    showExcel() {
      if (this.selectedDancerQuery !== "") {
        this.songs = []
        axios
          .get('http://127.0.0.1:8000/showExcel/')
          .then(response => (this.songs = response.data))
      } else {
        alert("No .xlsx file found.")
      }
    }
  }
}
</script>
