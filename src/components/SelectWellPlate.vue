<script setup lang="ts">
import { reactive } from 'vue';

interface ComponentState {
  availableWellPlates: string[];
  selectWellPlateModel: string;
  errorMessage: string;
}

const emits = defineEmits(['selected-well-plate', 'clear-plate-data']);

const state = reactive<ComponentState>({
  availableWellPlates: ['96', '384'],
  selectWellPlateModel: '',
  errorMessage: '',
});

const selectWellPlate = (): void => {
  state.errorMessage = '';
  if(state.selectWellPlateModel === '') {
    state.errorMessage = 'Please insert a value';
    return;
  } 

  if(state.selectWellPlateModel === '') {
    state.errorMessage = 'Please insert a value';
    return;
  } 

  if(state.selectWellPlateModel.length > 3 || state.selectWellPlateModel.length < 2) {
    state.errorMessage = 'Invalid number';
    return;
  }

  if(Number.isNaN(state.selectWellPlateModel)) {
    state.errorMessage = 'Please insert a number';
    return;
  }

  if(!state.availableWellPlates.includes(state.selectWellPlateModel)) {
    state.errorMessage = 'Available numbers 96 or 384';
    return;
  }
  
  emits('selected-well-plate', state.selectWellPlateModel);
};

const clearData = (): void => {
  state.selectWellPlateModel = '';
  state.errorMessage = '';
  emits('clear-plate-data');
};
</script>

<template>
  <div class="container">
    <div class="wrapper">
      <form class="select-well-form" action="" @submit.prevent="selectWellPlate">
        <label for="well-plate" class="text-uppercase text-700">Select well plate:</label>
        <input type="text" v-model="state.selectWellPlateModel" class="select-well-input" />
        <button type="submit" class="text-capitalize select-well-button submit">select</button>
        <button type="button" class="text-capitalize select-well-button clear" @click="clearData">clear</button>
        <div v-show="state.errorMessage" class="error-message select-well-error">{{ state.errorMessage }}</div>
      </form>
    </div>
  </div>
</template>

<style scoped>
.select-well-form {
  position: relative;
  width: 100%;
  background-color: var(--background-color-1);
  padding: 20px;
}

label[for="well-plate"] {
  margin: 0 20px 0 0;
}

.select-well-button {
  margin: 0 0 0 20px;
}

.select-well-error {
  margin: 20px 0 0;
}
</style>