<template>
  <v-app>
    <v-card class="pa-2">
      <v-card-title>
        <v-text-field v-model="search" append-icon="mdi-magnify" label="Search" single-line hide-details></v-text-field>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn dark v-bind="attrs" v-on="on">New Entry</v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">New Dancer</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12">
                    <v-text-field v-model="editedItem.name" label="Name"></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="close">
                Cancel
              </v-btn>
              <v-btn color="blue darken-1" text @click="save">
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-card-title>
      <v-data-table :headers="headers" :items="dancers" item-key="ID" :items-per-page="-1" :search="search">
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
  name: 'DancerView',

  data: () => ({
    dancers: [{ "ID": 0, "Name": "Long" },],
    search: '',
    dialog: false,
    editedItem: {
        name: '',
      },
      defaultItem: {
        name: '',
      },
  }),

  computed: {
    headers() {
      return [
        {
          text: 'DancerID',
          value: 'ID'
        },
        {
          text: 'Name',
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
      .get('http://127.0.0.1:8000/dancer')
      .then(response => (this.dancers = response.data))
  }
}
</script>

<style>

</style>