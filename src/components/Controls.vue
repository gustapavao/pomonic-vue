<script setup>
import { ref, computed } from 'vue';
import PlayButton from '/icons/play.svg';
import PauseButton from '/icons/pause.svg';

const props = defineProps({
  running: Boolean,
  toggle: Function,
  stage: String,
  changeStage: Function,
  setShortbreak: Function,
  setLongBreak: Function,
  setFocusTime: Function,
  timeOver: Function
});

const isModalOpen = ref(false);
const shortBreak = ref(5);
const longBreak = ref(15);
const focusTime = ref(25);

const buttonIcon = computed(() => props.running ? PauseButton : PlayButton);

function next() {
  props.timeOver()
}

const mainButtonStyle = computed(() => ({
  background: `var(--${props.stage}-bg-button-color)`
}));

const sideButtonStyle = computed(() => ({
  background: `var(--${props.stage}-third-color)`
}));

function openMenu() {
  isModalOpen.value = true;
}

function closeMenu() {
  isModalOpen.value = false;
}

function saveSettings() {
  props.setShortbreak(shortBreak.value);
  props.setLongBreak(longBreak.value);
  props.setFocusTime(focusTime.value);
  closeMenu();
}
</script>


<template>
  <div class="buttons-container">
    <button :style="[sideButtonStyle]" @click=openMenu class="side-button"><img src="/icons/dots.svg" alt="menu"></button>
    <button :style="[mainButtonStyle]" @click=props.toggle id="start-button"><img :src="buttonIcon" alt="play"></button>
    <button :style="[sideButtonStyle]" @click=next class="side-button"><img src="/icons/fast-forward.svg" alt="fast-forward"></button>
  </div>

  <!-- Modal -->
  <div v-if="isModalOpen" class="modal-overlay">
    <div class="modal-content">
      <button @click="closeMenu" class="close-button">&times;</button>
      <h2>Settings</h2>
      <label for="focus-time">Study Time:</label>
      <input v-model.number="focusTime" type="number" id="short-break" min="1" />

      <label for="short-break">Short Break:</label>
      <input v-model.number="shortBreak" type="number" id="short-break" min="1" />

      <label for="long-break">Long Break:</label>
      <input v-model.number="longBreak" type="number" id="long-break" min="1" />

      <button id="save-button" @click="saveSettings">Save</button>
    </div>
  </div>
</template>

<style scoped>
 .buttons-container {
   margin-top: 2rem;
 }

button {
  cursor: pointer;
}

#start-button {
  background: var(--normal-bg-button-color);
  padding: 2rem 2.5rem;
  width: 8rem;
  height: 6rem;
  border-radius: 2rem;
  border: 0;
}

.side-button {
  margin: 0 0.3rem;
  background: var(--normal-third-color);
  padding: 1.5rem;
  border-radius: 1.5rem;
  border: 0;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: var(--normal-bg-button-color);
  padding: 2rem;
  border-radius: 8px;
  width: 300px;
  position: relative;
  font-size: 1.6rem;
  color: white;
  font-weight: 800;
}

.close-button {
  position: absolute;
  top: 10px;
  right: 10px;
  background: none;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
}

h2 {
  margin-top: 0;
}

label {
  display: block;
  margin-top: 1rem;
}

input {
  width: 100%;
  padding: 0.5rem;
  margin-top: 0.5rem;
}

#save-button {
  margin: 1.5rem 0;
  padding: 1rem 4rem;
  color: white;
  background: var(--normal-bg-button-color);
  border: none;
  border-radius: 2rem;
}
</style>