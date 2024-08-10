<script setup>
  import BrainIcon from '/icons/brain.svg'
  import BreakIcon from '/icons/coffee.svg'
</script>

<template>
  <div class="modus">
    <img id="modus-img" :src="BrainIcon" alt="mode"> <b id="modus-text">Focus</b>
  </div>
</template>

<style scoped>
.modus{
  color: var(--normal-second-color);
  background: var(--normal-third-color);
  padding: 0.5rem 1rem;
  border-radius: 0.8rem;
  margin-bottom: 2rem;
  width: 12rem;
  font-size: 30px;
  text-align: center;
  font-family: 'Roboto Flex';
}


</style>
