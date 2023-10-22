<script setup>
import { defineExpose, onMounted, ref } from 'vue'
import * as echarts from 'echarts';
import DataHistory from '../WelcomeComponents/DataHistory.vue';

// 基于准备好的dom，初始化echarts实例
const props = defineProps(["data"])
let myChart;
onMounted(() => {
    myChart = echarts.init(document.getElementById('mychart'));
})
const data = ref([])
const createChart = () => {
    if (!props.data || !props.data.days) return;
    data.value = [{
        label: "区块",
        value: props.data?.block
    },
    {
        label: "井号",
        value: props.data?.tuneNo
    },
    {
        label: "浓度",
        value: props.data?.concentration + ' ppm'
    },
    {
        label: "温度",
        value: props.data?.temprature + ' ℃'
    },
    {
        label: "产量",
        value: props.data?.yield + ' 万方/天'
    },
    {
        label: "产水量",
        value: props.data?.water + ' 方/天'
    },
    {
        label: "工具尺寸",
        value: props.data?.size
    },
    {
        label: "入井天数",
        value: props.data?.water + ' 天'
    },
    {
        label: "溶后体积",
        value: props.data?.remain || 0 + ' 方'
    }]
    // 绘制图表
    myChart.setOption({
        title: {
            text: '溶解预测数据'
        },
        tooltip: {},
        xAxis: {
            data: props.data?.daysArr
        },
        yAxis: {
            type: "value"
        },
        series: [
            {
                name: '溶解比例',
                type: 'line',
                data: props.data?.dataArr,
                smooth: true
            }
        ]
    });
}
defineExpose({ createChart })
</script>

<template>
    <div class="chart-container">
        <div class="chart-box">
            <div id="mychart" style="width:100%;min-height: 100px;"></div>
            <a-divider direction="vertical" />
            <DataHistory />
        </div>
        <div class="detail-box">
            <a-divider direction="horizontal" />
            <div class="detail-container">
                <a-descriptions :data="data" title="预测数据" :align="{ label: 'right' }" />
            </div>
        </div>

    </div>
</template>

<style lang="less" scoped>
.chart-container {
    margin: 0 20px;
    flex-grow: 1;
    display: flex;
    border: 1px solid #cacaca;
    flex-direction: column;

    .chart-box {
        margin: 0 20px;
        display: flex;
        flex-grow: 1;
    }

    .detail-box {
        height: 200px;
        padding: 20px;
        width: 100%;
    }
}
</style>