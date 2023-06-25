<script setup lang="ts">
import { PropType } from 'vue';
import { IWell, IWellPlateConfig } from '../../interfaces/experiment.interface';

const props = defineProps({
  selectedWellPlateConfig: {
    type: Object as PropType<IWellPlateConfig>,
    required: true,
  },
  rowValue: {
    type: Array as PropType<Array<IWell | null>>,
    required: true,
  },
  rowIndex: {
    type: Number,
    required: true,
  }
});

const capitalA: number = 65;

const getLetter = (index: number): string => {
  return String.fromCharCode(capitalA + index);
};
</script>

<template>
  <div :class="['row-container', { 'last': props.rowIndex === selectedWellPlateConfig.rows - 1 }, { 'first': props.rowIndex === 0 }]">
    <span class="row-index row-index-start">{{ `${props.rowIndex + 1}:${getLetter(props.rowIndex)}` }}</span>
    <slot></slot>
    <span class="row-index row-index-end">{{ `${getLetter(props.rowIndex)}:${props.rowIndex + 1}` }}</span>
  </div>
</template>

<style scoped>
.row-container {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
}

.row-index {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px;
  width: var(--column-width);
  height: var(--column-height);
}

.row-index-start {
  border-right: 1px solid var(--border-color-1);
}

.row-index-end {
  padding: 20px 20px 20px 40px;
}

.indexes {
  position: relative;
  width: 100%;
  display: flex;
}
</style>