<template>
  <v-app>
    <v-card class="pa-2">
      <v-card-title>
        <v-text-field v-model="search" append-icon="mdi-magnify" label="Search" single-line hide-details></v-text-field>
        <v-spacer></v-spacer>
        <v-btn dark @click="newItem">New Entry</v-btn>
      </v-card-title>
      <v-data-table :headers="headers" :items="sequences" item-key="ID" :items-per-page="-1" :search="search">
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
  name: 'SequenceView',

  data: () => ({
    sequences: [{
    "ID": 0,
    "SongID": 3,
    "Start": 65,
    "End": 101,
    "Description": "Chorus 1",
    "Comment": "Start/End from RDG Stuttgart",
    "LastTimeInRDG": "2022-10-07 00:00:00",
    "Artist": "Sunmi",
    "Title": "24 hours"
  },],
    search: '',
  }),

  computed: {
    headers() {
      return [
        {
          text: 'ID',
          value: 'ID'
        },
        {
          text: 'SongID',
          value: 'SongID',
        },
        {
          text: 'Artist (EXTERNAL)',
          value: 'Artist'
        },
        {
          text: 'Title (EXTERNAL)',
          value: 'Title'
        },
        {
          text: 'Start',
          value: 'Start'
        },
        {
          text: 'End',
          value: 'End'
        },
        {
          text: 'Description',
          value: 'Description'
        },
        {
          text: 'Comment',
          value: 'Comment'
        },
        {
          text: 'LastTimeInRDG',
          value: 'LastTimeInRDG'
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
      .get('http://127.0.0.1:8000/sequence')
      .then(response => (this.sequences = response.data))
  }
}
</script>

<style>

</style>