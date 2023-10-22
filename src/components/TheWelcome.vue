<script setup>
import DataForm from './WelcomeComponents/DataForm.vue';
import lineChart from './WelcomeComponents/LineChart.vue';
import { ref, nextTick, provide } from 'vue'
import { useStorage } from '@vueuse/core'
import { STORAGE_KEY } from '@/config/config'
const params = ref({});
const line = ref(null)
// 接受数据变化，调用
function recieveData(data) {
  // 调用计算过程
  const result = calculateData(data)
  params.value = result;
  nextTick(() => {
    line.value.createChart()
  })
}
// 计算公式，暂时模拟
function calculateData(data){
  const days = data?.days, obj = {...data};
  obj.daysArr = Array.from({ length: days }, (v, i) => i + 1);
  obj.dataArr = Array.from({ length: days }, (v, i) => {
    return Math.floor(100 / (i + 1))
  });
  return obj
}
// 接受保存历史记录的方法
function saveData(data){
  // 调用计算过程
  const result = calculateData(data);
  const state = useStorage(STORAGE_KEY,[])
  result.stamp = new Date().getTime()
  state.value.push(result)  
}
// 直接生成曲线图，用于历史数据生成
function simpleCreate(data){
  params.value = data;
  nextTick(() => {
    line.value.createChart()
  })  
}
provide("createChart",recieveData)
</script>

<template>
  <div class="main">
    <DataForm @submit="recieveData" @save="saveData" />
    <lineChart :data="params" ref="line" />
  </div>
</template>
<style scoped lang="less">
.main {
  display: flex;
  flex:1;
}
</style>
