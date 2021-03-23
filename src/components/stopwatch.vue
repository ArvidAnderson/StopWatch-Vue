<template>
  <div class="flex min-h-screen">
    <div class="m-auto">
      <div class="text-center p-5 text-4xl">
        <span v-if="hours > 0">{{ convertZero(hours) + ':' }}</span>
        <span>{{ convertZero(minutes) + ':' }}</span>
        <span>{{ convertZero(seconds.toFixed(0)) }}</span>
      </div>
      <div>
        <button
          v-on:click="startStopWatch()"
          class="focus:outline-none text-white bg-green-500 p-4 py-3 px-6 rounded-full"
        >
          Start
        </button>
        <button
          v-on:click="stopStopWatch()"
          class="focus:outline-none text-white bg-red-500 p-4 py-3 px-6 rounded-full"
        >
          Stop
        </button>
        <button
          v-if="active != true" 
          v-on:click="clearStopWatch()"
          class="focus:outline-none text-white bg-blue-500 p-4 py-3 px-6 rounded-full"
        >
          Clear
        </button>
        <button
          v-if="active == true" 
          v-on:click="lapStopWatch()"
          class="focus:outline-none text-white bg-blue-500 p-4 py-3 px-6 rounded-full"
        >
          Lap
        </button>
      </div>
      <div class="mt-5 text-center text-xl">
          <ul>
            <li v-for="(lap, index) in laps" :key="index" class="m-3">
              <span v-on:click="laps.splice(index, 1)">
                {{convertZero(lap.hours) + ':' + convertZero(lap.minutes) + ':' + convertZero(lap.seconds.toFixed(0))}}
              </span>
            </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "stopwatch",
  data() {
    return {
      seconds: 0,
      minutes: 0,
      hours: 0,
      active: false,
      laps: [],
      interval: null,
    };
  },
  mounted() {
    console.log("MOUNTED");
    this.interval = setInterval(this.updateStopWatch, 10);
    console.log(this.laps)
  },
  methods: {
    convertZero(type) {
      var convStr = type.toString();
      if (convStr.length == 2) {
        return convStr;
      } else {
        return "0" + convStr;
      }
    },
    startStopWatch() {
      this.active = true;
    },
    stopStopWatch() {
      this.active = false;
    },
    clearStopWatch() {
      this.hours = 0;
      this.minutes = 0;
      this.seconds = 0;
    },
    lapStopWatch() {
      this.laps.push({hours: this.hours, minutes: this.minutes, seconds: this.seconds})
      console.log(this.laps)
    },
    updateStopWatch() {
      if (this.active == true) {
        this.seconds += 0.01;
      }
      if (this.seconds >= 60) {
        this.seconds = 0;
        this.minutes += 1;
      }
      if (this.minutes >= 60) {
        this.minutes = 0;
        this.hours++;
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
