<script setup>
import { onMounted, ref } from 'vue'
import * as echarts from 'echarts';
import DataHistory from '../WelcomeComponents/DataHistory.vue';

// 基于准备好的dom，初始化echarts实例
const props = defineProps(["data"]);
let myChart;
onMounted(() => {
    myChart = echarts.init(document.getElementById('mychart'));
    // 节点被点击时
    myChart.on("click",param =>{
        let i = param.dataIndex;
        if(i){ //这里需要排除掉前边计算过程中push的0
            // 这里做data.value内部值的修改，主要是剩余物相关的数据，因为每个节点不一样
        }
        
    })
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
            type:'category', 
            boundaryGap: false,      
            data: props.data?.daysArr
        },
        yAxis: {
            type: "value"
        },
        series: [
            {
                name: '溶解比例',
                type: 'line',
                symbolSize: 8,
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
            <div id="mychart" style="min-width:400px;min-height: 100px;"></div>
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