<script setup>
import {ref ,inject} from 'vue'
import { useStorage } from '@vueuse/core'
import { STORAGE_KEY } from '@/config/config'
import moment from 'moment'

const current = ref(null)
const state = useStorage(STORAGE_KEY,[])
const simpleCreate = inject("createChart")
const showDate = (stamp) => {
    return moment(stamp).format('YYYYMMDD HH:mm:ss')
}
function historyChange(value){
    simpleCreate(state.value[value])
}
</script>
<template>
    <div class="his_container">
        <h3>历史数据</h3>
        <div class="his-list">
            <a-radio-group direction="vertical" v-model="current" @change="historyChange">
                <a-radio v-for="(el,index) in state" :value="index">{{ el.block }}区块-{{ el.tuneNo }}号井{{ showDate(el.stamp) }}</a-radio>
        </a-radio-group>           
        </div>
    </div>
</template>
<style scope>
.his_container{
    width:300px;
}
</style>