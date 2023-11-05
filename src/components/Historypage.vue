
<template>
  <!-- <div>
    <div class="his-list">
      <a-radio-group direction="vertical" v-model="current" @change="historyChange2">
                <a-radio v-for="(el,index) in sycslist" :value="index">{{ el.wd }}℃-{{ el.nd }}ppm{{ el.mj }}c㎡</a-radio>
      </a-radio-group> 
    </div>

    <div class="chart-container">
        <div class="chart-box">
            <div id="mychart" style="width:100%;min-height: 100px;"></div>
            <a-divider direction="vertical" />
            <DataHistory />
        </div>
        <div class="detail-box">
            <a-divider direction="horizontal" />
            <div class="detail-container">
                <a-descriptions :data="sycslist[selIndex]" title="预测数据" :align="{ label: 'right' }" />
            </div>
        </div>
    </div>
  
  </div> -->
</template>

<script>
import sycs from '@/public/datajson1.json'
import sysj from '@/public/datajson2.json'
    export default{
        data(){
            return{
                sycslist:[],//实验参数
                showcharts:[],//显示的实验数据
                selIndex:0 //当前选中的数据
            }
         },
         methods(){
            loadsycs()
            {
              sycslist = sycs;
            }
            //创建图表
            initlinecharts()
            {
            }
            //获取当前选中参数的实验数据
            getsysjbyid(sycsid)
            {
              var currchart=[];
              sysj.sysj.forEach(e => {
                  if(e.sycsid==sycsid)
                  {
                      currchart.push(e);
                  }
              });
              return currchart;
            }
            //选择改变
            historyChange2(value){}
         }
    }
</script>


<style scoped>
</style>
