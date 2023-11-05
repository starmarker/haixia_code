<script setup>
import { ref, inject } from 'vue'
import { useStorage } from '@vueuse/core'
import { STORAGE_KEY } from '@/config/config'
import moment from 'moment'
import sycs from '@/public/datajson1.json'
import sysjH from '@/public/data-90-150.json'
import sysjL from '@/public/data-30-75.json'

const current = ref(null)
const current2 = ref(null)
const state = useStorage(STORAGE_KEY, [])
const state2 = sycs.sycs
const simpleCreate = inject("createChart")
const showDate = (stamp) => {
    return moment(stamp).format('YYYYMMDD HH:mm:ss')
}
function historyChange(value) {

    var s = state.value[value];
    simpleCreate(state.value[value])
}
//选择数据改变@参数为选中数据的index
function historyChange2(value) {
    //选中的实验参数
    var cursycs = state2[value];

    //选中的实验数据
    var cu = getsysjlist(cursycs.id, cursycs.wd.split(',')[1]);
    var data2 = {
        data: cursycs
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

/**zhx */
</script>
<template>
    <div class="his_container">
        <a-tabs default-active-key="2">
            <a-tab-pane key="1" title="历史数据1">
                <a-scrollbar style="height:calc(100vh - 500px);overflow: auto;">
                <div class="his-list">
                    <a-radio-group direction="vertical" v-model="current" @change="historyChange">
                        <a-radio v-for="(el, index) in state" :value="index">{{ el.block }}区块-{{ el.tuneNo }}号井{{
                            showDate(el.stamp) }}</a-radio>
                    </a-radio-group>
                </div>
            </a-scrollbar>
            </a-tab-pane>
            <a-tab-pane key="2" title="历史数据2">
                <a-scrollbar style="height:calc(100vh - 500px);overflow: auto;">
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