<template>
  <div class="countdown">
    <div v-if="!running">
      <label>How long would you like to study for?</label>
      <span class="text-center pl-2 block my-2"> 
        <input class="set-time w-1/6 bg-gray-100" type="number" min="0" v-model="enteredTime" placeholder="Enter a Time in Minutes">
        minutes
      </span>
      <button class="custom-button bg-green-400" type="button" @click="startNewCountdown()">Start</button>
    </div>
    <!-- bind this to a variable holding minutes -->
    <div class="flex flex-col" v-if="running">
      <span v-if="!countdownDone" class="coundown-number text-5xl">  
        {{ displayedTime }}
      </span>
      <span class="text-red-400 text-5xl animate-pulse" v-if="countdownDone">
        DONE
      </span>
      <hr class="m-1">
      <div class="space-x-1" v-if="!countdownDone">
        <button class="custom-button bg-green-400" type="button" @click="resumeCountdown()">Play</button>
        <button class="custom-button bg-orange-500" type="button" @click="pauseCountdown()">Pause</button>
      </div>
      <button class="restart-button text-xs" type="button" @click="restartCountdown()">Restart</button> 
      <button class="restart-button text-xs" type="button" @click="changeTime()">Change Time</button>
    </div>

    
    <!-- input in minutes, I need to have minutes and seconds and maybe even hours? no, start with minutes -->
    
  </div>
</template>

<script>

export default {
  name: 'Countdown',
  props: {
  },
  data() {
    return {
      runningTime: null,
      //startTime: null,
      enteredTime: 0,
      buttonState: 'Start',

      running: false,
      remainingTime: null,
      countdownDone: false,
      timerChekc: null,
      pauseTime: null,
    }
  },
  computed: {
    // countdownInSeconds(){
    //   return this.runningTime * 60;
    // },
    displayedTime(){
      let calcSeconds = this.runningTime % 60;
      let calcMinutes= (this.runningTime - (calcSeconds)) / 60;
      let displayMinutes = calcMinutes.toString();
      let displaySeconds = calcSeconds.toString();
      
      // could seperate these out as a funciton .. 
      // assign minutes text to appear
      switch (displayMinutes.length){
        case 2:
          break;
        case 1:
          displayMinutes = "0" + displayMinutes;
          break;
        case 0:
          displayMinutes = "00";
          break;
        default:
          break;
      }
      // assin seconds text to appear
      switch (displaySeconds.length){
        case 2:
          break;
        case 1:
          displaySeconds = "0" + displaySeconds;
          break;
        case 0:
          displaySeconds = "00";
          break;
        default:
          break;
      }
      return displayMinutes + ":" + displaySeconds;
    },
  },
  methods: {
    startNewCountdown(){
      if(this.enteredTime == 0){
        alert("time can't be zero");
      }
      else{
        this.runningTime = this.enteredTime * 60;
        this.runCountdown(this.runningTime);
        this.countdownDone = false;
      }
    },
    pauseCountdown(){
      clearTimeout(this.timerCheck);
      //this.startTime = this.runningTime;
    },
    resumeCountdown(){
      this.runCountdown(this.runningTime);
    },
    restartCountdown(){
      clearTimeout(this.timerCheck);
      this.startNewCountdown();
    },
    runCountdown(timerTime) {
      // assign the remainingTime to the current seconds to allow for allow for computed properties to access this loop
      // on every loop set the running time equal to the current iteration of the loop
      this.runningTime = timerTime;
      this.running = true;
      // if the timer time is still above 0, keep running the loop; will also need to check to see if time has been
      // paused, reset or changed
      if (timerTime > 0){
        timerTime--;
        this.timer(timerTime);
      }
      else{
        this.countdownDone = true;
      }   
    },
    changeTime(){
      this.pauseCountdown();
      this.running = false;
    },
    timer(time){
      this.timerCheck = setTimeout(() => {this.runCountdown(time)}, 1000);
    },
    

  }
}
</script>

<style scoped>
.countdown{
  border-radius: 3.5rem;
  @apply shadow-lg;
  @apply p-5; 
  @apply font-serif;
  @apply font-extrabold;
}

.custom-button{
  @apply border-black;
  @apply rounded-lg;
  @apply px-2;
  @apply py-1;
}


</style>


// TODO: if pause need to stop the current setTimeout Completely! 
// this will be sitting in a card that has similar style to the overall cards
// DIST: could be awesome to throw a distraction killer note taking section in here