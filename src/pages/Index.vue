<template>
  <div
    class="flex flex-column justify-around items-center"
    :style="{ width: window.width + 'px', height: window.height + 'px' }"
  >
    <h1 class="f1 f-headline-m f-headline-l white">
      {{ hours | timeFormat }}:{{ minutes | timeFormat
      }}<span class="f3 self-top">{{ seconds | timeFormat }}</span>
    </h1>
    <div class="flex flex-row">
      <Buttons
        v-if="!started"
        title="5"
        subtitle="minutes"
        @click.native="addTime(0, 5)"
      />
      <Buttons
        v-if="!started"
        title="30"
        subtitle="minutes"
        @click.native="addTime(0, 30)"
      />
      <Buttons
        v-if="!started"
        title="1"
        subtitle="hour"
        @click.native="addTime(1, 0)"
      />
    </div>
    <div class="flex flex-row">
      <Buttons
        v-if="started && !paused"
        size="125px"
        title="Pause"
        @click.native="pauseTimer()"
      />
      <Buttons
        v-else
        size="125px"
        :title="paused ? 'Continue' : 'Start'"
        @click.native="startTimer()"
      />
      <Buttons
        v-if="started"
        size="125px"
        title="Stop"
        @click.native="stopTimer()"
      />
    </div>
  </div>
</template>
<script>
import Buttons from "../components/Buttons.vue";
import audioFile from "../assets/alarm.mp3";
export default {
  components: { Buttons },
  data() {
    return {
      minutes: 0,
      hours: 0,
      seconds: 0,
      expires_in: null,
      started: false,
      paused: false,
      interval: null,
      window: {
        width: 0,
        height: 0,
      },
    };
  },
  created() {
    window.addEventListener("resize", this.handleResize);
    this.handleResize();
  },

  destroyed() {
    window.removeEventListener("resize", this.handleResize);
  },
  methods: {
    handleResize() {
      this.window.width = window.innerWidth;
      this.window.height = window.innerHeight;
    },
    stopTimer() {
      clearInterval(this.interval);
      this.paused = false;
      this.hours = 0;
      this.minutes = 0;
      this.seconds = 0;
      this.started = false;
    },
    pauseTimer() {
      clearInterval(this.interval);
      this.paused = true;
    },
    playSound() {
      var audio = new Audio(audioFile); // path to file
      audio.play();
    },
    startTimer() {
      this.started = true;
      this.paused = false;
      this.expires_in = this.minutes * 60 + this.hours * 60 * 60 + this.seconds;
      this.interval = setInterval(() => {
        console.log(`${this.hours} ${this.minutes}`);
        if (this.expires_in === 0) {
          console.log("SE SALIO DEL INTERVALO");
          clearInterval(this.interval);
          this.playSound();
          this.started = false;
        } else {
          this.expires_in -= 1;
          let d = Number(this.expires_in);
          this.hours = Math.floor(d / 3600);
          this.minutes = Math.floor((d % 3600) / 60);
          this.seconds = Math.floor((d % 3600) % 60);
        }
      }, 1000);
    },
    addTime(hour, minutes) {
      console.log("ENTRO");
      let newMinutes = this.minutes + minutes;
      let newHour = this.hours + hour;
      if (newMinutes <= 59) {
        this.minutes += minutes;
      } else if (newMinutes == 60) {
        this.minutes = 0;
        if (this.hours < 24) {
          this.hours++;
        }
      }
      if (newHour <= 23) {
        this.hours += hour;
      }
    },
  },
  filters: {
    timeFormat(val) {
      if (val == 0) return "00";
      if (val < 10) {
        return `0${val}`;
      } else {
        return `${val}`;
      }
    },
  },
};
</script>