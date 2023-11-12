<script setup>
import { onMounted, ref, inject} from 'vue'
import * as echarts from 'echarts';
import DataHistory from '../WelcomeComponents/DataHistory.vue';

// 基于准备好的dom，初始化echarts实例
const props = defineProps(["data"]),{setDataIndex} = inject("setDataIndex");
let myChart;

onMounted(() => {
    myChart = echarts.init(document.getElementById('mychart'));
    // 节点被点击时
    myChart.on("click",param =>{
        let i = param.dataIndex;
        if(i){ //这里需要排除掉前边计算过程中push的0
            // 这里做data.value内部值的修改，主要是剩余物相关的数据，因为每个节点不一样
            setDataIndex(i)
        }       
    })
    window.addEventListener('resize',()=>{
        myChart.resize()

    })
})
const data = ref([])
const tooltipFormatter = (params) =>{
    if(!props.data || params.dataIndex<=0)return;
    let str = ''
    let item = props.data.datadetail[params.dataIndex-1];
    str +="时刻:"+item.time;
    if(item.in_box>0){
        str +="(放入真空)";
    }
    str+=' <br />';
    str +="反应前质量:"+item.weight_start+' <br />';
    str +="反应后质量:"+item.weight_end+' <br />';
    str +="反应时间:"+item.use_time+' <br />';
    str +="比表面积:"+item.area+' <br />';
    str +="溶解速率:"+item.speed+' <br />';
    str += `总溶解比例：${params.value}%`;
    return str;
}
const createChart = () => {
{
    // if (!props.data || !props.data.days) return;
    if(!props.data) return;
    if(!props.data.days){
        updateCharts(props.data);
    }else{
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
        label: "初始质量",
        value: props.data?.premg + ' mg'
    },
    {
        label: "表面积",
        value: props.data?.area + ' 平方厘米'
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
        tooltip:{
            formatter:tooltipFormatter
        },
        xAxis: {
            type:'category', 
            boundaryGap: false,      
            data: props.data?.daysArr
        },
        yAxis: {
            type: "value",
            axisLabel:{
                show:true,
                interval:'auto',
                formatter:'{value}%'
            }
        },
        series: [
            {
                name: '溶解比例',
                type: 'line',
                symbolSize: 12,
                data: props.data?.dataArr,
                smooth: true
            }
        ]
    });
    
}
}
}
function updateCharts(data2){
    data.value = [
    {
        label: "浓度",
        value: data2.data?.nd
    },
    {
        label: "温度",
        value: data2.data?.wd + ' ℃'
    },
    {
        label: "初始质量",
        value:  data2.data?.zl + ' mg'
    },
    {
        label: "表面积",
        value: data2.data?.mj + ' cm2'
    },
    {
        label: "其他",
        value: data2.data?.other
    }]
    // 绘制图表
    myChart.setOption({
        title: {
            text: '溶解数据'
        },
        tooltip:{
            formatter:tooltipFormatter
        },       
        xAxis: {
            type:'category', 
            boundaryGap: false,      
            data: data2.daysArr
        },
        yAxis: {
            type: "value",
            axisLabel:{
                show:true,
                interval:'auto',
                formatter:'{value}%'
            }
        },
        series: [
            {
                name: '溶解比例',
                type: 'line',
                symbolSize: 8,
                data: data2.dataArr,
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
            <div id="mychart" style="width:100%;height: calc(100vh - 400px)"></div>
            <a-divider direction="vertical" />
            <DataHistory />
        </div>
        <div class="detail-box">
            <a-divider direction="horizontal" />
            <div class="detail-container">
                <a-descriptions :data="data" title="实验参数" :align="{ label: 'right' }" />
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
        height:calc(100vh - 400px);
        margin: 0 20px;
        display: flex;
        flex-grow: 1;
    }

    .detail-box {
        box-sizing: border-box;
        height: 280px;
        padding: 20px;
        width: 100%;
    }
}
</style>