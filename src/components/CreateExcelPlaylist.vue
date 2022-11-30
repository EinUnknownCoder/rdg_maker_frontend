<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="6">
        <v-btn dark block @click="showExcel">Find/Show Excel</v-btn>
      </v-col>
      <v-col cols="12" md="6">
        <v-btn color="primary" block @click="createPlaylist">Create Playlist</v-btn>
      </v-col>
      <v-col cols="12" v-if="timestamps != 'Empty'">
        <v-textarea v-model="timestamps" readonly>
          <template v-slot:label>
            <div>
              Timestamps for YouTube Video Description
            </div>
          </template>
        </v-textarea>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <v-form>
          <v-row>
            <v-col cols="12" md="4">
              <v-switch v-model="includeCountdown" label="Countdown"></v-switch>
            </v-col>
            <v-col cols="12" md="4">
              <v-switch v-model="includeIntro" label="Intro (MONSTA X - LOVE)"></v-switch>
            </v-col>
            <v-col cols="12" md="4">
              <v-switch v-model="includeOutro" label="Outro (BTS - RUN)"></v-switch>
            </v-col>
            <v-col cols="12" md="3">
              <v-text-field v-model="preTime" label="Time before Dance Part" type="number"></v-text-field>
            </v-col>
            <v-col cols="12" md="3">
              <v-text-field v-model="postTime" label="Time after Dance Part" type="number"></v-text-field>
            </v-col>
            <v-col cols="12" md="3">
              <v-text-field v-model="fadeInTime" label="Fade In Time" type="number"></v-text-field>
            </v-col>
            <v-col cols="12" md="3">
              <v-text-field v-model="fadeOutTime" label="Fade Out Time" type="number"></v-text-field>
            </v-col>
          </v-row>
        </v-form>
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
              <th>Dancer</th>
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
              <td> {{ song.Dancer }} </td>
              <td> {{ song.Start }}</td>
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
    songs: [],
    includeIntro: false,
    includeOutro: false,
    includeCountdown: true,
    preTime: 10,
    postTime: 2,
    fadeInTime: 2,
    fadeOutTime: 2,
    timestamps: "Empty"
  }),

  computed: {
    fileProperties() {
      return Object.getOwnPropertyNames(this.file[0]['values'])
    }
  },

  methods: {
    showExcel() {
      this.songs = []
      axios
        .get('http://127.0.0.1:8000/showExcel')
        .then(response => (this.songs = response.data))
    },
    createPlaylist() {
      axios.post('http://127.0.0.1:8000/createExcelPlaylist', {
        countdown: this.includeCountdown,
        intro: this.includeIntro,
        outro: this.includeOutro,
        preTime: this.preTime,
        postTime: this.postTime,
        fadeInTime: this.fadeInTime,
        fadeOutTime: this.fadeOutTime
      }).then(response => (this.timestamps = response.data))
    }
  }
}
</script>
