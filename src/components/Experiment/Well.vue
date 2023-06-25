<script setup lang="ts">
import { PropType } from 'vue';
import { IWell } from '../../interfaces/experiment.interface';

const props = defineProps({
  wellValue: {
    type: Object as PropType<IWell | null>,
    required: true,
  },
  wellIndex: {
    type: Number,
    required: true,
  }
});
</script>

<template>
  <span class="column" :data-column-index="props.wellIndex + 1">{{ props.wellValue ? `${props.wellValue.sample}/${props.wellValue.reagent}` : '' }}</span>
</template>

<style scoped>
.column {
  position: relative;
  width: var(--column-width);
  height: var(--column-height);
  padding: 10px;
  background-color: var(--background-color-1);
  border-top: 1px solid var(--border-color-1);
  border-right: 1px solid var(--border-color-1);
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  font-size: 12px;
}

.row-container.first > .column:before,
.row-container.last > .column:after
{
  position: absolute;
  top: -45px;
  content: attr(data-column-index);
  font-size: 16px;
}

.row-container.last > .column:after {
  top: auto;
  bottom: -45px;
}

.row-container.last > .column {
  border-bottom: 1px solid var(--border-color-1);
}
</style>