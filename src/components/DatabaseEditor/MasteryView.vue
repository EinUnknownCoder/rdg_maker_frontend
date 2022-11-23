<template>
  <v-app>
    <v-card class="pa-2">
      <v-card-title>
        <v-text-field v-model="search" append-icon="mdi-magnify" label="Search" single-line hide-details></v-text-field>
        <v-spacer></v-spacer>
        <v-btn dark @click="newItem">New Entry</v-btn>
      </v-card-title>
      <v-data-table :headers="headers" :items="masteries" item-key="ID" :items-per-page="-1" :search="search">
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
  name: 'MasteryView',

  data: () => ({
    masteries: [{
    "ID": 0,
    "SequenceID": 15,
    "DancerID": 1,
    "Description": "Chorus 1",
    "Artist": "ENHYPEN",
    "Title": "Blessed-Cursed",
    "Name": "Laura"
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
          text: 'SequenceID',
          value: 'SequenceID',
        },
        {
          text: 'DancerID',
          value: 'DancerID'
        },
        {
          text: 'Description (EXTERNAL)',
          value: 'Description'
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
          text: 'Name (EXTERNAL)',
          value: 'Name'
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
      .get('http://127.0.0.1:8000/mastery')
      .then(response => (this.masteries = response.data))
  }
}
</script>

<style>

</style>