<script setup lang="ts">
const props = defineProps({
  experimentNum: {
    type: Number,
    required: true,
  },
});
</script>

<template>
  <div class="experiment-container">
    <h1>{{ `Experiment - ${props.experimentNum + 1}` }}</h1>
    <slot></slot>
  </div>
</template>

<style scoped>

.experiment-container {
  margin: 0 0 40px;
}

.experiment-container:last-child {
  margin: 0;
}
</style>