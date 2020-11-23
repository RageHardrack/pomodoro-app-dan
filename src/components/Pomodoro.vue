<template>
  <v-card class="mt-8 text-center">
    <v-tabs @change="changeCurrentTimer" v-model="currentTimer" grow>
      <v-tab v-for="timer in timers" :key="timer.name">
        {{ timer.name }}
      </v-tab>
    </v-tabs>

    <v-card class="pa-5" flat>
      <h1 class="display-4">{{ displayMinutes }}:{{ displaySeconds }}</h1>

      <v-btn-toggle class="mt-5" rounded>
        <v-btn class="pa-5" color="primary" @click="start">
          <v-icon class="white--text" small left
            >mdi-play-circle-outline</v-icon
          >
          <span> Inicio </span>
        </v-btn>
        <v-btn class="pa-5" color="error" @click="stop">
          <v-icon class="white--text" small left>
            mdi-stop-circle-outline
          </v-icon>
          <span> Pausa </span>
        </v-btn>
        <v-btn
          color="warning"
          @click="reset(timers[currentTimer].minutes)"
          :disabled="isRunning"
        >
          <v-icon class="white--text" small left>
            mdi-restart
          </v-icon>
          <span> Reiniciar </span>
        </v-btn>
      </v-btn-toggle>
    </v-card>

    <SettingsDialog
      :dialog="dialog"
      :closeDialog="closeDialog"
      :save="save"
      :timers="timers"
    />
  </v-card>
</template>

<script>
import SettingsDialog from "./SettingsDialog";

export default {
  name: "Pomodoro",
  components: { SettingsDialog },
  props: {
    dialog: {
      type: Boolean,
      required: true
    },
    closeDialog: {
      type: Function,
      required: true
    }
  },
  data() {
    return {
      isRunning: false,
      timerInstance: null,
      totalSeconds: 25 * 60,
      currentTimer: 0,
      timers: [
        { name: "Pomodoro", minutes: 25 },
        { name: "Descanso corto", minutes: 5 },
        { name: "Descanso largo", minutes: 10 }
      ]
    };
  },
  computed: {
    displayMinutes() {
      const minutes = Math.floor(this.totalSeconds / 60);
      return this.formatTime(minutes);
    },
    displaySeconds() {
      const seconds = this.totalSeconds % 60;
      return this.formatTime(seconds);
    }
  },
  methods: {
    formatTime(time) {
      if (time < 10) {
        return "0" + time;
      }
      return time.toString();
    },
    start() {
      this.stop();
      this.isRunning = true;
      this.timerInstance = setInterval(() => {
        if (this.totalSeconds <= 0) {
          this.stop();
          return;
        }
        this.totalSeconds -= 1;
      }, 1000);
    },
    stop() {
      this.isRunning = false;
      clearInterval(this.timerInstance);
    },
    reset(minutes) {
      this.stop();
      this.totalSeconds = minutes * 60;
    },
    changeCurrentTimer(num) {
      this.currentTimer = num;
      this.reset(this.timers[num].minutes);
    },
    save(updatedTimers) {
      this.timers = this.timers.map((timer, index) => {
        return { ...timer, minutes: parseInt(updatedTimers[index]) };
      });
      this.closeDialog();
    }
  }
};
</script>
