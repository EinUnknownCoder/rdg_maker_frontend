<template>
  <v-app>
    <v-card class="pa-2">

      <v-tabs vertical color="black">
        <v-tab>Dancer</v-tab>
        <v-tab>Song</v-tab>
        <v-tab>Sequence</v-tab>
        <v-tab>Mastery</v-tab>

        <v-tab-item>
          <v-card flat>
            <v-card-text>
              <DancerView />
            </v-card-text>
          </v-card>
        </v-tab-item>
        <v-tab-item>
          <v-card flat>
            <v-card-text>
              <SongView />
            </v-card-text>
          </v-card>
        </v-tab-item>
        <v-tab-item>
          <v-card flat>
            <v-card-text>
              <SequenceView />
            </v-card-text>
          </v-card>
        </v-tab-item>
        <v-tab-item>
          <v-card flat>
            <v-card-text>
              <MasteryView/>
            </v-card-text>
          </v-card>
        </v-tab-item>
      </v-tabs>
    </v-card>
  </v-app>
</template>

<script>
import DancerView from "./DatabaseEditor/DancerView";
import SongView from "./DatabaseEditor/SongView";
import SequenceView from "./DatabaseEditor/SequenceView";
import MasteryView from "./DatabaseEditor/MasteryView";

export default {
  name: 'EditDatabase',

  components: {
    DancerView,
    SongView,
    SequenceView,
    MasteryView
  },

  data: () => ({
    
  })
}
</script>

<style>

</style>