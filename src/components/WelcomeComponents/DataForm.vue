<script setup>
import { reactive, ref } from 'vue';
const data = reactive({
    block: '',//区块
    tuneNo: '',//井号
    concentration: 5,//浓度
    temprature: 30,//温度
    yield: 5,//产量
    water: 100,//产水量
    size: '',//公共尺寸
    days: 0, // 入井天数
    premg:0, //初始质量
    area:0  //比表面积
})

const blocks = ref([]), sizes = ref([]);
blocks.value = ["威远", "长宁", "致密气"];
sizes.value = ["2.3/8\"", "2.7/8\"", "3.1/2\""];
const emit = defineEmits(['submit','save'])
const handleSubmit = () => {
    emit('submit', data)
}
const saveRecord = () => {
    emit('save', data)
}
</script>

<template>
    <div class="data-form">
        <a-form :model="data" :style="{ width: '300px' }" :label-width="140" auto-label-width size="small"
            @submit="handleSubmit">
            <a-form-item field="block" label="区块名">
                <a-select v-model="data.block" placeholder="区块名">
                    <a-option v-for="el in blocks" :value="el" :label="el"></a-option>
                </a-select>
            </a-form-item>
            <a-form-item field="tuneNo" label="井号">
                <a-input v-model="data.tuneNo" placeholder="井号" />
            </a-form-item>
            <a-form-item field="concentration" label="浓度">
                <a-input-number v-model="data.concentration" placeholder="浓度" >
                    <template #suffix>
                        wt%
                    </template>                   
                </a-input-number>
            </a-form-item>
            <a-form-item field="temprature" label="温度">
                <a-input-number v-model="data.temprature" placeholder="温度" :min="-70" :max="300">
                    <template #suffix>
                        ℃
                    </template>
                </a-input-number>
            </a-form-item>
            <a-form-item field="yield" label="产量">
                <a-input-number v-model="data.yield" placeholder="产量" :min="0">
                    <template #suffix>
                        万方/天
                    </template>               
                </a-input-number>
            </a-form-item>
            <a-form-item field="water" label="产水量">
                <a-input-number v-model="data.water" placeholder="产水量" :min="0">
                    <template #suffix>
                        方/天
                    </template>
                </a-input-number>
            </a-form-item>
            <a-form-item field="size" label="工具尺寸">
                <a-select v-model="data.size" placeholder="请选择尺寸">
                    <a-option v-for="el in sizes" :value="el" :label="el"></a-option>
                </a-select>
            </a-form-item>
            <a-form-item field="days" label="入井天数">
                <a-input-number v-model="data.days" placeholder="入井天数" :min="1" />
            </a-form-item>
            <a-form-item field="premg" label="初始质量mg">
                <a-input-number v-model="data.premg" placeholder="mg" :min="1"/>
            </a-form-item>
            <a-form-item field="area" label="表面积">
                <a-input-number v-model="data.area" placeholder="c㎡" :min="1" />
            </a-form-item>
        </a-form>
        <a-space direction="vertical" fill class="btn-grop">
            <a-button @click="handleSubmit" type="primary" long>计算</a-button>
            <a-button @click="saveRecord" type="primary" long>保存数据</a-button>
        </a-space>
    </div>
</template>

<style>
.data-form {
    width: 330px;
    padding: 10px;
    border: 1px solid #cacaca;
}
</style>