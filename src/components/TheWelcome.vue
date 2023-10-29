<script setup>
import DataForm from './WelcomeComponents/DataForm.vue';
import lineChart from './WelcomeComponents/LineChart.vue';
import ProgressImage from './WelcomeComponents/ProgressImage.vue';
import { ref, nextTick, provide ,computed} from 'vue'
import { useStorage } from '@vueuse/core'
import { STORAGE_KEY } from '@/config/config'
const params = ref({
});
const line = ref(null)
// 初始数据节点，用于点击后展示示意图
const dataIndex = ref(0)
// 接受数据变化，调用
function recieveData(data) {
  // 调用计算过程
  const result = calculateDataNew(data)
  params.value = result;
  nextTick(() => {
    console.log(result);
    line.value.createChart()
  })
}
// 计算公式，暂时模拟
function calculateData(data) {

  // const days = data?.days, obj = {...data};
  // obj.daysArr = Array.from({ length: days }, (v, i) => i + 1);
  // obj.dataArr = Array.from({ length: days }, (v, i) => {
  //   return Math.floor(100 / (i + 1))
  // });
  // return obj
  const days = data?.days, obj = { ...data };

  var y1 = data.premg;//剩余质量

  var lineX = [];
  var lineY = [];

  var m = data.premg;//初始质量
  var s = data.area;//面积
  var t = data.temprature;//温度
  var p = data.concentration;//浓度
  var rjtime = 0; //时刻 
  var y11 = 0;//本轮剩余质量
  //计算总溶解时间
  var totaltime = getTotaltime(m, s, t, p);

  while (y1 > 0) {
    if (y1 == data.premg) {
      // 第1个小时
      var b0 = -44995.2044536;
      var b1 = -766.115552270;
      var b2 = 91.3860355110;
      var b3 = 10869.2319180;
      var b4 = -176.690867170;
      var b5 = 1.10564580741;
      y11 = b0 + b1 * data.temprature + b2 * Math.sqrt(m) + b3 * Math.sqrt(data.temprature) + b4 * Math.sqrt(data.concentration) + b5 * Math.pow(data.temprature, 2);
      //如果质量小于0,结束时间为总时间；
      if (y11 < 0) {
        rjtime = totaltime;
      }
    }
    else {
      // 第2个小时开始
      var b0 = 560496603.358;
      var b1 = 64026.2553447;
      var b2 = 2310111.82804;
      var b3 = -81088.7302091;
      var b4 = -84.1948864608;
      var b5 = 802.149130453;
      var r1 = -85273760.0627;
      var r2 = -12041034.2882;
      var r3 = -4643041.76603;
      var r4 = 1646.98684041;
      var l1 = 2317246.72199;
      var l2 = -4784.91488980;
      var b11 = -5.36349428489;
      var b33 = 58.3220550541;
      var b15 = 0.387051008360;
      var b25 = -140.884291766;
      var b35 = -0.675159174097;
      var b45 = -29.9482587510;
      var b12 = -374.711415683;

      y11 = b0 + b1 * m + b2 * s + b3 * t + b4 * p + b5 * rjtime + r1 * Math.log10(m) + r2 * Math.log(s) + r3 * Math.log10(t) + r4 * Math.log10(rjtime)
        + l1 * Math.sqrt(t) + l2 * Math.sqrt(rjtime) + b11 * Math.pow(m, 2) + b33 * Math.pow(t, 2) + b15 * m * rjtime
        + b25 * s * rjtime + b35 * t * rjtime + b45 * p * rjtime + b12 * m * s;

      if (y11 < 0) {
        rjtime = totaltime;
      }
    }

    //溶解率 %
    var rjl = 1.0 - y11 / data.premg;
    lineX.push(rjtime);
    lineY.push(rjl * 100);
    y1 = y11;
    m = y11;//下一轮初始质量等于本次剩余质量
    rjtime++;
  }
  obj.daysArr = lineX;
  obj.dataArr = lineY;

  return obj;
}

function calculateDataNew(data) {
  const days = data?.days, obj = { ...data };

  var y1 = data.premg;//剩余质量

  var lineX = [];
  var lineY = [];

  var m = data.premg;//初始质量
  var s = data.area;//面积
  var T = data.temprature;//温度
  var p = data.concentration;//浓度
  var rjtime = 1; //时刻 
  var y11 = 0;//本轮剩余质量
  //计算总溶解时间
  var totaltime = getTotaltime(m, s, T, p);

  lineX.push(0);
  lineY.push(0);
  while (y1 > 0) {
    y11 = residualmass1(rjtime, totaltime, m);
    //溶解率 %
    var rjl = 1.0 - y11 / data.premg;
    lineX.push(rjtime);
    lineY.push(rjl * 100);
    y1 = y11;
    m = y11;//下一轮初始质量等于本次剩余质量
    rjtime++;
  }
  obj.daysArr = lineX;
  obj.dataArr = lineY;

  return obj;
}
//拟合反应总时间:初始质量m,表面积s,温度T，浓度p,
function getTotaltime(m, s, T, p) {

  var a = 2.43977 * Math.pow(10, -4) * m;
  var b = 0.0992851 * s;
  var c = 0.968106 * T;
  var d = 2.05339 * (Math.pow(10, -3)) * Math.pow(T, 2);
  var e = 71.557 * Math.log(T);
  var f = 12.4135 * Math.log(p);
  var g = 0.0543982 * T * p;
  //反应总时间t1
  var t1 = 260.212 + a - b + c - d - e - f + g;

  if (t1 <= 0) {
    //算数异常
    return 0;
  }
  return t1;
}
//单调拟合1:时刻t,总拟合时间t1,初始质量m
function residualmass1(t, t1, m) {
  var yt;
  if (t >= 0 && t <= 2.47) {
    yt = m - 127.5 * Math.pow(t, 2);
  }
  else if (t > 2.47 && t < (t1 - 1.33)) {
    yt = ((945.0458 - m) * (t - 2.47)) / (t1 - 3.8) + m - 777.8648;
  }
  else if (t >= (t1 - 1.33) && t <= t1) {
    yt = 125.7 * (t1 - t);
  }
  else {
    yt = 0;
  }
  return yt;
}
//单调拟合2：
function residualmass2() {

}
// 接受保存历史记录的方法
function saveData(data) {
  // 调用计算过程
  const result = calculateData(data);
  const state = useStorage(STORAGE_KEY, [])
  result.stamp = new Date().getTime()
  state.value.push(result)
}
// 直接生成曲线图，用于历史数据生成
function simpleCreate(data) {
  params.value = data;
  nextTick(() => {
    line.value.createChart()
  })
}
function setDataIndex(index){
  console.log(index);
  dataIndex.value = index
}
const showProgress = computed(()=>{
  let arr = params.value?.dataArr||[0]
  return arr[dataIndex.value];
})
provide("createChart", recieveData)
provide("setDataIndex", {dataIndex,setDataIndex})
</script>

<template>
  <div class="main">
    <DataForm @submit="recieveData" @save="saveData" />
    <lineChart :data="params" ref="line" />
    <ProgressImage :progess="showProgress" /> 
  </div>
</template>
<style scoped lang="less">
.main {
  box-sizing: border-box;
  display: flex;
  flex: 1;
}
</style>
