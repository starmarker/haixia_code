<script setup>
import { ref, inject } from 'vue'
import { useStorage } from '@vueuse/core'
import { STORAGE_KEY } from '@/config/config'
import moment from 'moment'
import sycs from '@/public/datajson1.json'
import sysjH from '@/public/data-90-150.json'
import sysjL from '@/public/data-30-75.json'
// import fs from 'fs'

const current = ref(null)
const current2 = ref(null)
const state = useStorage(STORAGE_KEY, [])
const state2 = sycs.sycs
const simpleCreate = inject("createChart")
const showDate = (stamp) => {
    return moment(stamp).format('YYYYMMDD HH:mm:ss')
}
function historyChange(value) {
    current.value = value
    simpleCreate(state.value[current.value])
}
//选择数据改变@参数为选中数据的index
function historyChange2(value) {
    //选中的实验参数
    current2.value = value
    var cursycs = state2[current2.value];

    //选中的实验数据
    var cu = getsysjlist(cursycs.id, cursycs.wd.split(',')[1]);
    var data2 = {
        data: cursycs,
        datadetail:cu
    };

    var lineX = [0];
    var lineY = [0];
    cu.forEach(item => {
        //溶解率 %
        var rjl = 1.0 - item.weight_end / cursycs.zl;
        lineX.push(item.time);
        lineY.push(rjl * 100);
    });
    data2.daysArr = lineX;
    data2.dataArr = lineY;

    simpleCreate(data2);
}
//获取实验参数对应的实验数据
function getsysjlist(sycsid, wd) {

    var currchart = [];
    var sysj = sysjH;
    if (wd < 90) {
        sysj = sysjL;
    }
    sysj.datas.forEach(e => {
        if (e.sycsid == sycsid) {
            currchart.push(e);
        }
    });
    return currchart;
}
//读取历史实验参数
function gethistorylist() {

}
// 切换tab
function tabChange(){
    current.value = null;
    current2.value = null;
}

/**zhx */
</script>
<template>
    <div class="his_container">
        <a-tabs default-active-key="2" @change="tabChange">        
            
            <a-tab-pane key="1" title="历史预测数据">
                <a-scrollbar style="height:calc(100vh - 370px);overflow: auto;">
                <div class="his-list">
                    <a-radio-group direction="vertical" v-model="current" @change="historyChange">
                        <a-radio v-for="(el, index) in state" :value="index">{{ el.block }}区块-{{ el.tuneNo }}号井{{
                            showDate(el.stamp) }}</a-radio>
                    </a-radio-group>
                </div>
            </a-scrollbar>
            </a-tab-pane>
             <a-tab-pane key="2" title="溶解数据库">
                <a-scrollbar style="height:calc(100vh - 370px);overflow: auto;">
                <div class="his-list">
                    <a-radio-group direction="vertical" v-model="current2" @change="historyChange2">
                        <a-radio v-for="(el, index) in state2" :value="index">{{ el.nd }}-{{ el.wd }}℃-{{ el.other
                        }}c㎡</a-radio>
                    </a-radio-group>
                </div>
                </a-scrollbar>
            </a-tab-pane>
        </a-tabs>


    </div>
</template>
<style scope>
.his_container {
    width: 300px;
}
</style>