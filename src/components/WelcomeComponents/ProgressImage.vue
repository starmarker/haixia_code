<script setup>
import { computed } from 'vue'
const props = defineProps({
    progess: {
        type: Number,
        default: 0
    }
})
const imgSrc = computed(()=>{
    let progress = props.progess <0 ?0:props.progess>100?100:props.progess
    const rate = Math.round(progress / 10)
    console.log(rate,progress);
    return new URL(`../../assets/imgs/${rate}.png`,import.meta.url).href;
})

</script>

<template>
    <div class="images-container">
        <h3>溶解效果图</h3>
        <a-image :src="imgSrc"></a-image>
    </div>
</template>

<style lang="less">
  .images-container {
    width:200px;
    padding: 10px;
    border: 1px solid #cacaca;
  } 
</style>