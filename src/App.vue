<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row>
          <v-col sm="8" offset-sm="2">
            <Pomodoro :dialog="dialog" :closeDialog="closeDialog" />
          </v-col>
          <v-btn @click="dialog = true" color="secondary" dark top right fab>
            <v-icon>mdi-cog-outline</v-icon>
          </v-btn>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Pomodoro from "@/components/Pomodoro";

export default {
  name: "App",

  components: { Pomodoro },

  data: () => ({
    dialog: false
  }),
  methods: {
    closeDialog() {
      this.dialog = false;
    }
  }
};
</script>
