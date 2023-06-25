<script setup lang="ts">
import { ref } from 'vue';
import SelectWellPlate from './components/SelectWellPlate.vue';
import { IWellPlateConfig, IWell } from './interfaces/experiment.interface';
import Experiment from './components/Experiment/Experiment.vue'
import Plate from './components/Experiment/Plate.vue'
import WellRow from './components/Experiment/WellRow.vue'
import Well from './components/Experiment/Well.vue'

const wellPlateConfig: { [key: string]: IWellPlateConfig } = {
  '96': { rows: 8, columns: 12 },
  '384': { rows: 16, columns: 24 },
};
const wellPlate = ref<string>('');
const selectedWellPlateConfig = ref<IWellPlateConfig>({
  rows: 8,
  columns: 12,
});
const samples = ref<string[][]>([['Sam 1', 'Sam 2', 'Sam 3'], ['Sam 1', 'Sam 3', 'Sam 4']]);
const reagents = ref<string[][]>([['Reag X', 'Reag Y', 'Reag Z'], ['Reag Y', 'Reag Z']]);
const experimentNumReplicates = ref<number[]>([1, 3]);
const loadingResults = ref<boolean>(false);
const results = ref<IWell[][][][]>([]);

const generatePlate = (selectedWellPlate: IWellPlateConfig, expNum: number, samplesExp: string[], reagentsExp: string[], numRepetitions: number): IWell[][][] => {
  var plate = [];
  // SET WELL PLATE ROWS START VALUE
  var rowArr = new Array(selectedWellPlate.rows).fill(new Array(selectedWellPlate.columns).fill(null));
  var rAddIndex = 0;
  for(var sIndex = 0; sIndex <= samplesExp.length - 1; sIndex++) {
    var sample = samplesExp[sIndex];
    var columnArr = new Array(selectedWellPlate.columns).fill(null);
    var cAddIndex = 0;
    for(var regIndex = 0; regIndex <= reagentsExp.length - 1; regIndex++) {
      var reagent = reagentsExp[regIndex];
      for(var repIndex = 0; repIndex <= numRepetitions - 1; repIndex++) {
        // REPLACE COLUMNS FROM THE BEGINING
        columnArr[cAddIndex] = {sample, reagent, experiment: expNum + 1};
        cAddIndex++;
      }
    }
    // REPLACE ROWS FROM THE BEGINING
    rowArr[rAddIndex] = columnArr;
    rAddIndex++;
  }
  plate.push(rowArr);
  return plate;
}

const generateExperimentsReport = (wellPlate: string, expSamplesArr: string[][], expReagentsArr: string[][], numRepArr: number[]): IWell[][][][] => {
  try {
    var selectedWellPlate = wellPlateConfig[wellPlate];
    var numExperiments = expSamplesArr.length;
    var resultExp = [];
    for(var exp = 0; exp <= numExperiments - 1; exp++) {
      // UNIQUE SAMPLES
      var samplesExp = [ ...new Set(expSamplesArr[exp]) ];
      // UNIQUE REAGENTS
      var reagentsExp = [ ...new Set(expReagentsArr[exp]) ];
      // NUMBER OF REPETITIONS FOR SAMPLE REAGENT COMBINATION
      var numRepetitions = numRepArr[exp];
      // GENERATE PLATE
      var plate = generatePlate(selectedWellPlate, exp, samplesExp, reagentsExp, numRepetitions);
      // ADD PLATE TO EXPERIMENT
      resultExp.push(plate);
    }
    return resultExp;
  } catch(error) {
    console.error(error);
    return [];
  }
}

const handleSelectedWellPlate = (value: string): void => {
  loadingResults.value = true;
  wellPlate.value = value;
  selectedWellPlateConfig.value = wellPlateConfig[wellPlate.value];
  results.value = generateExperimentsReport(value, samples.value, reagents.value, experimentNumReplicates.value);
  loadingResults.value = false;
};

const handleClearData = (): void => {
  if(!results.value.length) {
    return;
  }
  results.value = [];
};

const addWrapperClass = () => {
  return wellPlate.value == '384';
};
</script>

<template>
  <SelectWellPlate @selected-well-plate="handleSelectedWellPlate" @clear-plate-data="handleClearData" />
  <div class="container">
    <div :class="['wrapper', { 'full-width': addWrapperClass() }]">
      <div v-if="loadingResults" class="loading">Loading results ...</div>
      <div v-else class="results-container">
        <Experiment v-for="(experiment, eIndex) in results" :experiment-num="eIndex">
          <Plate v-for="(plate, pIndex) in experiment" :experiments-count="experiment.length" :plate-value="plate" :plate-index="pIndex">
            <WellRow v-for="(row, rIndex) in plate" :selected-well-plate-config="selectedWellPlateConfig" :row-value="row" :row-index="rIndex">
              <Well v-for="(well, wIndex) in row" :well-value="well" :well-index="wIndex" />
            </WellRow>
          </Plate>
        </Experiment>
      </div>
    </div>
  </div>
</template>

<style scoped>
.wrapper.full-width {
  width: 100%;
}
</style>
