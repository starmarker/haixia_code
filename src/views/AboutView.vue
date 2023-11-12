<template>
    <div class="his_list">
        <a-tabs default-active-key="2">
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
    <div>
      <table>
        <thead>
          <tr>
            <th>时刻(h)</th>
            <th>反应前质量(mg)</th>
            <th>反应后质量(mg)</th>
            <th>质量变化(mg)</th>
            <th>反应时间(h)</th>
            <th>比表面积(cm2)</th>
            <th>腐蚀速率(mg·h-1·cm-2)</th>
          </tr>
        </thead>
        <tbody>
          <!-- <tr v-for="(el in )" -->
        </tbody>
      </table>
    </div>
</template>
<script setup>
import { ref, inject } from 'vue'
import { useStorage } from '@vueuse/core'
import { STORAGE_KEY } from '@/config/config'
import moment from 'moment'
import sycs from '@/public/datajson1.json'
import sysjH from '@/public/data-90-150.json'
import sysjL from '@/public/data-30-75.json'

const current2 = ref(null)
const state2 = sycs.sycs
// const state = ref(null)

//选择数据改变@参数为选中数据的index
function historyChange2(value) {
    //选中的实验参数
    var cursycs = state2[value];

    //选中的实验数据
    var cu = getsysjlist(cursycs.id, cursycs.wd.split(',')[1]);
    cu;
    // var data2 = {
    //     data: cursycs,
    //     datadetail:cu
    // };

    // var lineX = [0];
    // var lineY = [0];
    // cu.forEach(item => {
    //     //溶解率 %
    //     var rjl = 1.0 - item.weight_end / cursycs.zl;
    //     lineX.push(item.time);
    //     lineY.push(rjl * 100);
    // });
    // data2.daysArr = lineX;
    // data2.dataArr = lineY;

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
</script>

<style scoped>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
.his_list{
  width: 350px;
  border: 2px solid #cacaca;
}
.his_container {
    width: 100%;
}
</style>
