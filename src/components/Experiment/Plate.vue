<script setup lang="ts">
const props = defineProps({
  experimentsCount: {
    type: Number,
    required: true,
  },
  plateValue: {
    type: Array,
    required: true,
  },
  plateIndex: {
    type: Number,
    required: true,
  }
});
</script>

<template>
  <div class="experiment-container">
    <div class="plate-container">
      <h2 v-if="experimentsCount > 1">{{ `Plate - ${props.plateIndex + 1}` }}</h2>
      <div class="plate">
        <slot></slot>
      </div>
    </div>
  </div>
</template>

<style>
.plate-container {
  position: relative;
  width: 100%;
  padding: 20px;
}

.plate-container h2 {
  position: relative;
  width: 100%;
  border-bottom: 1px solid var(--border-color-1);
  padding: 0 0 20px;
}

.plate {
  --column-width: 96px;
  --column-height: 96px;
  padding: 40px 0;
}
</style>