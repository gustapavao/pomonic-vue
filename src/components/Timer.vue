<script setup>
import {computed} from "vue";

const props = defineProps({
  formattedTime: String,
  stage: String
});

const timeColor = computed(() => ({
  color: `var(--${props.stage}-second-color)`
}));
</script>

<template>
  <section :style="[timeColor]" class="time" id="time">{{ props.formattedTime }}</section>
</template>

<style scoped>

.time{
  width: auto;
  height: auto;
  max-width: 280px;
  font-family: 'Roboto Flex', sans-serif;
  font-size: 256px;
  line-height: 200px;
  font-weight: normal;
  text-align: center;
}
</style>