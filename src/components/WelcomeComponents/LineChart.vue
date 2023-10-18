<script setup>
import { defineExpose } from 'vue'
import * as echarts from 'echarts';

// 基于准备好的dom，初始化echarts实例
const props = defineProps(["data"])

const createChart = () => {
    if (!props.data || !props.data.days) return;
    let myChart = echarts.init(document.getElementById('mychart'));
    const days = props.data?.days;
    const arr = Array.from({ length: days }, (v, i) => i + 1);

    const data1 = Array.from({ length: days }, (v, i) => {
        return Math.floor(100 / (i+1))
    });
    const data2 = Array.from({ length: days }, (v, i) => {
        return Math.floor(100 / (i + 2))
    });
    // 绘制图表
    myChart.setOption({
        title: {
            text: '溶解预测数据'
        },
        tooltip: {},
        xAxis: {
            data: arr
        },
        yAxis: {
            type: "value"
        },
        series: [
            {
                name: '溶解比例',
                type: 'line',
                data: data1,
                smooth: true
            },
            {
                name: '预测数据',
                type: 'line',
                data: data2,
                smooth: true
            }
        ]
    });
}
defineExpose({createChart})
</script>

<template>
    <div class="chart-container">
        <div id="mychart" style="width:100%"></div>
    </div>
</template>

<style lang="less" scoped>
.chart-container {
    flex-grow: 1;
    display: flex;
}
</style>