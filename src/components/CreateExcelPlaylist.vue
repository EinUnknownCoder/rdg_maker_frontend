<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="5">
        <v-btn dark block @click="showExcel">Find/Show Excel</v-btn>
      </v-col>
      <v-col cols="12" md="5">
        <v-btn color="primary" block @click="createPlaylist" :loading="loading">Create Playlist</v-btn>
      </v-col>
      <v-col cols="12" md="2">
        <v-btn block color="secondary" @click="resetCreateButton">Reset</v-btn>
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
            <v-col cols="12" md="2">
              <v-btn color="light-blue" block @click="presetRDGStuttgart">Stuttgart</v-btn>
            </v-col>
            <v-col cols="12" md="2">
              <v-btn dark color="#0D47A1" block @click="presetRDGKarlsruhe">Karlsruhe</v-btn>
            </v-col>
            <v-col cols="12" md="2">
              <v-btn dark color="black" block @click="presetRPDMunich">Munich</v-btn>
            </v-col>
            <v-col cols="12" md="2">
              <v-btn dark color="#FB8C00" block @click="presetARDGGoeppingen">Göppingen</v-btn>
            </v-col>
            <v-col cols="12" md="2">
              <v-btn dark color="blue-grey" block @click="presetRPDPforzheim">Pforzheim</v-btn>
            </v-col>
            <v-col cols="12" md="2">
              <v-btn dark color="#8E24AA" block @click="presetRDGDuesseldorf">Düsseldorf</v-btn>
            </v-col>

            <v-col cols="12" md="2">
              <v-btn color="light-blue" block @click="presetRDGStuttgartHalftime">Stuttgart: Halftime</v-btn>
            </v-col>
            <v-col cols="12" md="2">
              <v-btn disabled dark color="#0D47A1" block @click="presetRDGKarlsruhe">Karlsruhe</v-btn>
            </v-col>
            <v-col cols="12" md="2">
              <v-btn disabled dark color="black" block @click="presetRPDMunich">Munich</v-btn>
            </v-col>
            <v-col cols="12" md="2">
              <v-btn disabled dark color="#FB8C00" block @click="presetARDGGoeppingen">Göppingen</v-btn>
            </v-col>
            <v-col cols="12" md="2">
              <v-btn disabled dark color="blue-grey" block @click="presetRPDPforzheim">Pforzheim</v-btn>
            </v-col>
            <v-col cols="12" md="2">
              <v-btn disabled dark color="#8E24AA" block @click="presetRDGDuesseldorf">Düsseldorf</v-btn>
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

            <v-col cols="12" md="3">
              <v-slider v-model="playlistAmount" hint="Amount of Playlists (WIP)" thumb-label="always" :thumb-size="20" min="1" max="10" persistent-hint></v-slider>
            </v-col>
            <v-col cols="12" md="3">
              <v-select label="Countdown Voice" :items="countdownVoices" v-model="countdownVoice"></v-select>
            </v-col>
            <v-col cols="12" md="3">
              <v-select label="Countdown Length" :items="countdownLengths" v-model="countdownLength"></v-select>
            </v-col>
            <v-col cols="12" md="3">
              <v-select label="Cover Image" :items="coverImages" v-model="coverImage"></v-select>
            </v-col>

            <v-col cols="12" md="3">
              <v-switch v-model="randomizePlaylist" label="Randomize Playlist"></v-switch>
            </v-col>
            <v-col cols="12" md="3">
              <v-switch label="Backend Conformation" v-model="backendConformation"></v-switch>
            </v-col>
            <v-col cols="12" md="3">
              <v-switch label="10 Second Silence at the end" v-model="tenSecondSilenceAtEnd"></v-switch>
            </v-col>
            <v-col cols="12" md="3">
              <v-text-field label="File Name" v-model="fileName"></v-text-field>
            </v-col>

            <v-col cols="12" md="3">
              <v-switch label="Check YT URLs" v-model="checkYTURL"></v-switch>
            </v-col>
            <v-col cols="12" md="3">
              <v-text-field label="Start Checking YT URL from" v-model="checkYTURLPosition"></v-text-field>
            </v-col>
            <v-col cols="12" md="3">
              <v-switch label="Remove Dancer Text in Timestamp" v-model="removeDancer"></v-switch>
            </v-col>
            <v-col cols="12" md="3">
              <v-switch disabled label="WIP"></v-switch>
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
              <td> <a :href="song.URL + '&t=' + song.Start" target="_blank">{{ song.URL }}</a></td>
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
    countdownVoices: ["Salli"],
    countdownVoice: "Salli",
    countdownLengths: [3, 5],
    countdownLength: 3,
    coverImages: ["RDGMaker", "VAO", "KPopperStuttgart", "RDGStuttgart", "RDGKarlsruhe", "RPDMunich", "ARDGGoeppingen", "KBlast", "ComicConStuttgart", "PforzheimRPD", "RDGDDorf", "RDGStuttgart2"],
    coverImage: "RDGMaker",
    playlistAmount: 1,
    randomizePlaylist: true,
    backendConformation: true,
    tenSecondSilenceAtEnd: false,
    fileName: "",
    checkYTURL: true,
    removeDancer: false,
    checkYTURLPosition: 1
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
        countdownLength: this.countdownLength,
        coverImage: this.coverImage,
        playlistAmount: this.playlistAmount,
        randomizePlaylist: this.randomizePlaylist,
        backendConformation: this.backendConformation,
        tenSecondSilenceAtEnd: this.tenSecondSilenceAtEnd,
        fileName: this.fileName,
        checkYTURL: this.checkYTURL,
        removeDancer: this.removeDancer,
        checkYTURLPosition: this.checkYTURLPosition
      }).then(response => (this.timestamps = response.data))
    },
    resetCreateButton() {
      this.timestamps = "Ready to create Playlist!"
    },
    presetVAO() {
      this.includeCountdown = false
      this.countdownCrossfade = true
      this.includeIntro = true
      this.includeOutro = true
      this.preTime = 10
      this.postTime = 2
      this.fadeInTime = 2
      this.fadeOutTime = 2
      this.countdownVoice = "Salli"
      this.countdownLength = 3
      this.coverImage = "VAO"
      this.randomizePlaylist = true
      this.backendConformation = false
    },
    presetRDGStuttgart() {
      this.includeCountdown = true
      this.countdownCrossfade = true
      this.includeIntro = false
      this.includeOutro = false
      this.preTime = 8
      this.postTime = 2
      this.fadeInTime = 2
      this.fadeOutTime = 2
      this.countdownVoice = "Salli"
      this.countdownLength = 3
      this.coverImage = "RDGStuttgart"
      this.randomizePlaylist = true
      this.backendConformation = true
      this.fileName= "2025-00_RDG_Stuttgart_V"
      this.playlistAmount = 4
    },
    presetRDGKarlsruhe() {
      this.includeCountdown = true
      this.countdownCrossfade = true
      this.includeIntro = false
      this.includeOutro = false
      this.preTime = 0
      this.postTime = 1
      this.fadeInTime = 2
      this.fadeOutTime = 2
      this.countdownVoice = "Salli"
      this.countdownLength = 3
      this.coverImage = "RDGKarlsruhe"
      this.randomizePlaylist = true
      this.backendConformation = true
      this.playlistAmount = 4
      this.fileName = "2025-00_RDG_Karlsruhe_V"
      this.removeDancer = false
      this.tenSecondSilenceAtEnd = true
    },
    presetRPDMunich() {
      this.includeCountdown = true
      this.countdownCrossfade = true
      this.includeIntro = false
      this.includeOutro = false
      this.preTime = 0
      this.postTime = 0
      this.fadeInTime = 2
      this.fadeOutTime = 2
      this.countdownVoice = "Salli"
      this.countdownLength = 3
      this.coverImage = "RPDMunich"
      this.randomizePlaylist = false
      this.backendConformation = false,
      this.fileName = "2025-00_RPD_Munich_V"
    },
    presetARDGGoeppingen() {
      this.includeCountdown = true
      this.countdownCrossfade = true
      this.includeIntro = false
      this.includeOutro = false
      this.preTime = 2
      this.postTime = 1
      this.fadeInTime = 2
      this.fadeOutTime = 2
      this.countdownVoice = "Salli"
      this.countdownLength = 3
      this.coverImage = "ARDGGoeppingen"
      this.randomizePlaylist = true
      this.backendConformation = true
      this.fileName = "2025-00_ARDG_Göppingen_V"
      this.playlistAmount = 4
    },
    presetRPDPforzheim() {
      this.includeCountdown = true
      this.countdownCrossfade = true
      this.includeIntro = false
      this.includeOutro = false
      this.preTime = 8
      this.postTime = 2
      this.fadeInTime = 2
      this.fadeOutTime = 2
      this.countdownVoice = "Salli"
      this.countdownLength = 3
      this.coverImage = "PforzheimRPD"
      this.randomizePlaylist = true
      this.backendConformation = true
      this.fileName = "2025-00_RPD_Pforzheim_V"
      this.playlistAmount = 4
    },
    presetRDGDuesseldorf() {
      this.includeCountdown = true
      this.countdownCrossfade = true
      this.includeIntro = false
      this.includeOutro = false
      this.preTime = 5
      this.postTime = 3
      this.fadeInTime = 2
      this.fadeOutTime = 2
      this.countdownVoice = "Salli"
      this.countdownLength = 3
      this.coverImage = "RDGDDorf"
      this.randomizePlaylist = true
      this.backendConformation = true
      this.fileName = "2025-00_RDG_DDorf_V"
      this.playlistAmount = 2
    },
    presetRDGStuttgartHalftime() {
      this.includeCountdown = true
      this.countdownCrossfade = false
      this.includeIntro = false
      this.includeOutro = false
      this.preTime = 0
      this.postTime = 0
      this.fadeInTime = 0
      this.fadeOutTime = 0
      this.countdownVoice = "Salli"
      this.countdownLength = 3
      this.coverImage = "RDGStuttgart2"
      this.randomizePlaylist = true
      this.backendConformation = true
      this.fileName = "2025-00_RDG_Stuttgart_Halftime_Performance"
      this.playlistAmount = 10
      this.randomizePlaylist = false
      this.backendConformation = false
      this.checkYTURL = false
    }
  }
}
</script>
