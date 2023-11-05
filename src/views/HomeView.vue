<script setup>
import TheWelcome from '@/components/TheWelcome.vue'
</script>

<template>
    <TheWelcome />
</template>
<style lang="less" scoped>
.container {
  height: 100vh;
  width: 100%;

  & :deep(.arco-layout-header),
  & :deep(.arco-layout-footer),
  & :deep(.arco-layout-sider-children),
  & :deep(.arco-layout-content) {
    display: flex;
    flex-direction: column;
    // justify-content: center;
    // color: var(--color-white);
    font-size: 16px;
    font-stretch: condensed;
  }


  & :deep(.arco-layout-header),
  & :deep(.arco-layout-footer) {
    height: 60px;
    padding: 10px;
  }

  & :deep(.arco-layout-header) {
    box-shadow: #eee 5px 0px 5px;
  }

  & :deep(.arco-layout-footer) {
    text-align: center;
  }

  & :deep(.arco-layout-content) {
    box-shadow: #eee -5px 0px 5px;
    padding: 10px;
  }
}
</style>
