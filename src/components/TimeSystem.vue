<script setup>

import Notification from "@/components/Notification.vue";
import Timer from "@/components/Timer.vue";
import Controls from "@/components/Controls.vue";
import {computed, ref, watch} from "vue";

const seconds = ref(1500);
const longBreak = ref(15);
const shortbreak = ref(5);
const focusTime = ref(25);
const running = ref(false);
const timerInterval = ref(null);
const formattedTime = computed(() => {
  const minutes = Math.floor(seconds.value / 60);
  const secs = seconds.value % 60;
  return `${minutes < 10 ? '0' + minutes : minutes} ${secs < 10 ? '0' + secs : secs}`;
});
let stage = 'normal';

function changeStage(newStage){
  stage = newStage;
  const rootStyle = getComputedStyle(document.documentElement);
  const backgroundColor = rootStyle.getPropertyValue(`--${newStage}-main-color`).trim();
  document.body.style.backgroundColor = backgroundColor;
}

const notify = ref(null);

let breaked = false;
function timeOver(){
  pause();
  if(stage === "normal"){
    if(breaked){
      changeStage('long')
      setSeconds(longBreak.value * 60)
      breaked = false;
      toggle();
    }else{
      changeStage('break')
      setSeconds(shortbreak.value * 60)
      breaked = true;
      toggle();
    }
  }else {
    changeStage('normal')
    setSeconds(focusTime.value * 60)
    toggle();
  }

}

function setSeconds(value) {
  seconds.value = value;
}

function setLongBreak(value) {
  longBreak.value = value;
}

function setShortBreak(value) {
  shortbreak.value = value;
}

function setFocusTime(value) {
  focusTime.value = value;
}


function start() {
  running.value = true;
  timerInterval.value = setInterval(run, 1000);
}

function pause() {
  running.value = false;
  clearInterval(timerInterval.value);
}

function toggle() {
  if (running.value) {
    pause();
  } else {
    start();
  }
}

function run() {
  if (seconds.value > 0) {
    seconds.value--;
  } else {
    notify.value.notifyMe();
    timeOver()

  }
}

watch(running, (newValue) => {
  if (newValue === false) {
    clearInterval(timerInterval.value);
  }
});
</script>

<template>
    <Timer :formattedTime="formattedTime" :stage="stage"/>
    <Controls :toggle="toggle" :changeStage="changeStage"
              :running="running" :stage="stage"
              :setShortbreak="setShortBreak"
              :setLongBreak="setLongBreak"
              :setFocusTime="setFocusTime"
              :timeOver="timeOver"
    />
    <Notification ref="notify"/>
</template>

<style scoped>

</style>