<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="6">
        <v-btn dark block @click="showExcel">Find/Show Excel</v-btn>
      </v-col>
      <v-col cols="12" md="6">
        <v-btn color="primary" block @click="createPlaylist" :loading="loading">Create Playlist</v-btn>
      </v-col>
      <v-col cols="12" v-if="showTimestamps">
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
              <v-btn dark color="#8E24AA" block @click="presetVAO">VAO Preset</v-btn>
            </v-col>
            <v-col cols="12" md="4">
              <v-btn block @click="presetRDG">RDG Stuttgart Preset</v-btn>
            </v-col>
            <v-col cols="12" md="4">
              <v-btn block @click="showExcel" disabled>No more Presets</v-btn>
            </v-col>

            <v-col cols="12" md="3">
              <v-switch v-model="includeCountdown" label="Countdown"></v-switch>
            </v-col>
            <v-col cols="12" md="3">
              <v-switch :disabled="!includeCountdown" v-model="countdownCrossfade" label="Countdown Crossfade"></v-switch>
            </v-col>
            <v-col cols="12" md="3">
              <v-switch v-model="includeIntro" label="Intro (MONSTA X - LOVE)"></v-switch>
            </v-col>
            <v-col cols="12" md="3">
              <v-switch v-model="includeOutro" label="Outro (BTS - RUN)"></v-switch>
            </v-col>

            <v-col cols="12" md="3">
              <v-slider v-model="preTime" hint="Time before Dance Part" thumb-label="always" min="0" max="10" persistent-hint></v-slider>
            </v-col>
            <v-col cols="12" md="3">
              <v-slider v-model="postTime" hint="Time after Dance Part" thumb-label="always" min="0" max="10" persistent-hint></v-slider>
            </v-col>
            <v-col cols="12" md="3">
              <v-slider v-model="fadeInTime" hint="Fade In Time" thumb-label="always" min="0" max="10" persistent-hint></v-slider>
            </v-col>
            <v-col cols="12" md="3">
              <v-slider v-model="fadeOutTime" hint="Fade Out Time" thumb-label="always" min="0" max="10" persistent-hint></v-slider>
            </v-col>

            <v-col cols="12" md="6">
              <v-select label="Countdown Voice" :items="countdownVoices" v-model="countdownVoice"></v-select>
            </v-col>
            <v-col cols="12" md="6">
              <v-select label="Cover Image" :items="coverImages" v-model="coverImage"></v-select>
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
    countdownCrossfade: true,
    preTime: 10,
    postTime: 2,
    fadeInTime: 2,
    fadeOutTime: 2,
    timestamps: "Ready to create Playlist!",
    countdownVoices: ["Calm", "Neutral"],
    countdownVoice: "Neutral",
    coverImages: ["VAO", "KPopperStuttgart", "RDGStuttgart"],
    coverImage: "VAO"
  }),

  computed: {
    fileProperties() {
      return Object.getOwnPropertyNames(this.file[0]['values'])
    },
    loading() {
      if (this.timestamps == "Loading...") {
        return true
      } else {
        return false
      }
    },
    showTimestamps() {
      if (this.timestamps == "Loading..." || this.timestamps == "Ready to create Playlist!") {
        return false
      } else {
        return true
      }
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
      this.timestamps = "Loading..."
      axios.post('http://127.0.0.1:8000/createExcelPlaylist', {
        countdown: this.includeCountdown,
        countdownCrossfade: this.countdownCrossfade,
        intro: this.includeIntro,
        outro: this.includeOutro,
        preTime: this.preTime,
        postTime: this.postTime,
        fadeInTime: this.fadeInTime,
        fadeOutTime: this.fadeOutTime,
        countdownVoice: this.countdownVoice,
        coverImage: this.coverImage
      }).then(response => (this.timestamps = response.data))
    },
    presetVAO() {
      this.includeCountdown = false
      this.countdownCrossfade = false
      this.includeIntro = true
      this.includeOutro = true
      this.preTime = 10
      this.postTime = 2
      this.fadeInTime = 2
      this.fadeOutTime = 2
      this.countdownVoice = "Calm"
      this.coverImage = "VAO"
    },
    presetRDG() {
      this.includeCountdown = true
      this.countdownCrossfade = false
      this.includeIntro = false
      this.includeOutro = false
      this.preTime = 0
      this.postTime = 2
      this.fadeInTime = 2
      this.fadeOutTime = 2
      this.countdownVoice = "Calm"
      this.coverImage = "RDGStuttgart"
    }
  }
}
</script>
