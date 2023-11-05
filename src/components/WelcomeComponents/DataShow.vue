<script setup>

const props = defineProps(["detail"])


</script>

<template>
    <div class="detail-container" style="height: 340px;">
        <a-descriptions :data="data" title="预测数据" :align="{ label: 'right' }" />
    </div>
</template>