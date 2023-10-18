<script setup>
import DataForm from './WelcomeComponents/DataForm.vue';
import lineChart from './WelcomeComponents/LineChart.vue';
import {ref,nextTick} from 'vue'
const params = ref({});
const line = ref(null)
function recieveData( data ) {
  params.value = data;
  nextTick(()=>{

    line.value.createChart()
  })
}
</script>

<template>
  <div class="main">
    <DataForm @submit="recieveData" />
    <lineChart :data="params" ref="line" />
  </div>
</template>
<style scoped lang="less">
  .main {
    display: flex;

  }
</style>
