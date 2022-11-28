<template>
    <v-app>
        <v-row>
            <v-col cols="12" md="3">
                <v-data-table v-model="selectedDancer" :headers="headers" :items="dancer" item-key="ID" show-select
                    :items-per-page="-1" class="pa-2">
                </v-data-table>
            </v-col>
            <v-col cols="12" md="9">
                <v-row class="pa-2">
                    <v-col cols="12" md="6">
                        <v-btn @click="showPreview" block color="secondary">Show Preview</v-btn>
                    </v-col>
                    <v-col cols="12" md="6">
                        <v-btn @click="createPlaylist" block color="primary" :loading="playlistLoading"
                            :disabled="playlistLoading">Create Playlist</v-btn>
                    </v-col>
                </v-row>
                <v-row v-if="timestamps != ''">
                    <v-col cols="12">
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
                        <v-simple-table>
                            <template v-slot:default>
                                <thead>
                                    <tr>
                                        <th class="text-left">
                                            Title
                                        </th>
                                        <th class="text-left">
                                            Artist
                                        </th>
                                        <th class="text-left">
                                            Description
                                        </th>
                                        <th class="text-left">
                                            Dancer
                                        </th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="song in songs" :key="song.SequenceID">
                                        <td>{{ song.Title }}</td>
                                        <td>{{ song.Artist }}</td>
                                        <td>{{ song.Description }}</td>
                                        <td>{{ song.Dancer }}</td>
                                    </tr>
                                </tbody>
                            </template>
                        </v-simple-table>
                    </v-col>

                </v-row>

            </v-col>
        </v-row>
    </v-app>
</template>

<script>
import axios from 'axios';

export default {
    name: 'CreateVAOPlaylist',

    data: () => ({
        itemsPerPage: -1,
        headers: [
            {
                text: 'Dancer',
                value: 'Name'
            }
        ],
        selectedDancer: [],
        dancer: [],
        songs: [],
        timestamps: "Please create a playlist first..."
    }),

    methods: {
        showPreview() {
            if (this.selectedDancerQuery !== "") {
                this.songs = []
                axios
                    .get('http://127.0.0.1:8000/showPreview/' + this.selectedDancerQuery)
                    .then(response => (this.songs = response.data))
            } else {
                alert("Please select 1 or more dancers.")
            }
        },
        createPlaylist() {
            if (this.selectedDancerQuery !== "") {
                this.timestamps = "Loading...";
                axios
                    .get('http://127.0.0.1:8000/createPlaylist/' + this.selectedDancerQuery)
                    .then(response => (this.timestamps = response.data["Data"]));
            } else {
                alert("Please select 1 or more dancers.")
            }
        }
    },

    computed: {
        selectedDancerQuery() {
            let result = []
            if (this.selectedDancer.length > 0) {
                this.selectedDancer.forEach(element => {
                    result.push(element["ID"])
                });
            }
            return result.join(',');
        },
        playlistLoading() {
            if (this.timestamps == "Loading...") {
                return true
            } else {
                return false
            }
        }
    },

    mounted() {
        axios
            .get('http://127.0.0.1:8000/dancer')
            .then(response => (this.dancer = response.data))
    }
}
</script>

<style>

</style>