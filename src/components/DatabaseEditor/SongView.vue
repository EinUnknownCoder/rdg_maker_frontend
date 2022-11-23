<template>
  <v-app>
    <v-card class="pa-2">
      <v-card-title>
        <v-text-field v-model="search" append-icon="mdi-magnify" label="Search" single-line hide-details></v-text-field>
        <v-spacer></v-spacer>
        <v-btn dark @click="newItem">New Entry</v-btn>
      </v-card-title>
      <v-data-table :headers="headers" :items="songs" item-key="ID" :items-per-page="-1" :search="search">
        <template v-slot:[`item.actions`]="{ item }">
          <v-icon small class="mr-2" @click="editItem(item)">
            mdi-pencil
          </v-icon>
          <v-icon small @click="deleteItem(item)">
            mdi-delete
          </v-icon>
        </template>
      </v-data-table>
    </v-card>
  </v-app>
</template>

<script>
import axios from 'axios'

export default {
  name: 'SongView',

  data: () => ({
    songs: [{
    "ID": 0,
    "URL": "https://youtu.be/-rTHTKf-m98",
    "Artist": "(G)I-DLE",
    "Title": "LATATA",
    "Comment": null
  },],
    search: '',
  }),

  computed: {
    headers() {
      return [
        {
          text: 'SongID',
          value: 'ID'
        },
        {
          text: 'URL',
          value: 'URL',
          sortable: false,
          filterable: false,
        },
        {
          text: 'Artist',
          value: 'Artist'
        },
        {
          text: 'Title',
          value: 'Title'
        },
        {
          text: 'Comment',
          value: 'Comment'
        },
        {
          text: 'Actions',
          value: 'actions',
          sortable: false
        },
      ]
    },
  },

  methods: {
    newItem() {
      console.log("New Item");
    },
    editItem(item) {
      for (var property in item) {
        console.log(property + "=" + item[property]);
      }
    },
    deleteItem(item) {
      for (var property in item) {
        console.log(property + "=" + item[property]);
      }
    },
  },

  mounted() {
    axios
      .get('http://127.0.0.1:8000/song')
      .then(response => (this.songs = response.data))
  }
}
</script>

<style>

</style>