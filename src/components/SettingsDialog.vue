<template>
  <v-dialog v-model="dialog" persistent max-width="600px">
    <v-card>
      <v-card-title>
        <h1 class="headline">Configuración</h1>
      </v-card-title>

      <v-card-text>
        <v-text-field
          v-for="(timer, index) in updatedTimers"
          :key="index"
          label="Pomodoro"
          v-model="updatedTimers[index]"
          type="number"
        />
      </v-card-text>

      <v-card-actions>
        <v-btn color="primary" text @click="save(updatedTimers)">
          Save
        </v-btn>
        <v-btn color="error" text @click="closeDialog">
          Close
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: "SettingsDialog",
  props: {
    dialog: {
      type: Boolean,
      required: true
    },
    closeDialog: {
      type: Function,
      required: true
    },
    timers: {
      type: Array,
      required: true
    },
    save: {
      type: Function,
      required: true
    }
  },
  data() {
    return {
      updatedTimers: []
    };
  },
  mounted() {
    this.updatedTimers = this.timers.map(timer => {
      return timer.minutes;
    });
  }
};
</script>
