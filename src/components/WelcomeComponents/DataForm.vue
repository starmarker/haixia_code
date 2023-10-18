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
    days: 0 // 入井天数
})
const blocks = ref([]),sizes = ref([]);
blocks.value = ["区块1","区块2","区块三"];
sizes.value = ["尺寸1","尺寸2","尺寸3"];
const emit = defineEmits(['submit'])
const handleSubmit = () => {
    emit('submit',data)
}
</script>

<template>
    <div class="data-form">
        <a-form :model="data" :style="{ width: '300px' }" :label-width="140" auto-label-width size="small" @submit="handleSubmit">
            <a-form-item field="block" label="区块名">
                <a-select v-model="data.block" placeholder="区块名" >
                    <a-option v-for="el in blocks" :value="el" :label="el"></a-option>
                </a-select>
            </a-form-item>
            <a-form-item field="tuneNo" label="井号">
                <a-input v-model="data.tuneNo" placeholder="井号" />
            </a-form-item>
            <a-form-item field="concentration" label="浓度"  help="ppm">
                <a-input-number v-model="data.concentration" placeholder="浓度" />
            </a-form-item>
            <a-form-item field="temprature" label="温度"  help="℃">
                <a-input-number v-model="data.temprature" placeholder="温度"  :min="-20" :max="120" />
            </a-form-item>
            <a-form-item field="yield" label="产量"  help="万方/天">
                <a-input-number v-model="data.yield" placeholder="产量"  :min="0" />
            </a-form-item>
            <a-form-item field="water" label="产水量"  help="方/天">
                <a-input-number v-model="data.water" placeholder="产水量"  :min="0" />
            </a-form-item>
            <a-form-item field="size" label="公共尺寸">
                <a-select v-model="data.size" placeholder="区块名" >
                    <a-option v-for="el in sizes" :value="el" :label="el"></a-option>
                </a-select>
            </a-form-item>
            <a-form-item field="days" label="入井天数">
                <a-input-number v-model="data.days" placeholder="入井天数"  :min="1" />
            </a-form-item>
        </a-form>
        <a-button @click="handleSubmit" type="primary" long>计算</a-button>
    </div>
</template>

<style>
.data-form {
    width:330px;
    padding:10px;
    border:1px solid #cacaca;
}
</style>