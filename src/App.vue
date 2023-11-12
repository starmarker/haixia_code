<script setup>
import {useRouter, RouterView} from 'vue-router';
import {useStorage} from '@vueuse/core'
import { USER_INFO_KEY } from '@/config/config';
const keyPath = {
  "1":"/",
  "2":"/about",
  "3":"/about2",
  "4":"/about3",
};
const router = useRouter();
const changePage = (value) => {
  console.log(value,router);
  router.push(keyPath[value]||"/")
}
const $userInfo = useStorage(USER_INFO_KEY,{},sessionStorage)

//登录入口
function onlogin(){
    //登录
  router.push("/login")
  // loginstatus.value = true;
}
function loginout(){
  var result = confirm("确认退出吗？");
  if(result)
  {
    $userInfo.value = {}
    router.push("/login");
  }
}
</script>

<template>
  <a-layout class="container">
    <!-- 布局头部 -->
    <a-layout-header>
      <div>
        <a-image width="100" height="40" src="./logo.png" />
        <a-divider direction="vertical" />
        <span class="header-text">可溶堵塞器溶解预测软件</span>
 
        <a @click="onlogin" v-if="!$userInfo.username">请登录</a>
        <a @click="loginout" v-else>你好，{{ $userInfo.username }}! 点击登出</a>
        
      </div>
    </a-layout-header>
    <!-- 布局主体 -->
    <a-layout-content>
      <!-- 导航条 -->
      <a-menu mode="horizontal" :default-selected-keys="['1']" @menu-item-click="changePage">
        <a-menu-item key="1">首页</a-menu-item>
        <a-menu-item key="2">实验数据</a-menu-item>
        <!--a-menu-item key="3">菜单二</a-menu-item>
        <a-menu-item key="4">菜单三</a-menu-item-->
      </a-menu>
      <!-- 主体内容 -->
      <router-view></router-view>
    </a-layout-content>
    <!-- 布局页脚 -->
    <a-layout-footer>这里是页脚部分</a-layout-footer>
  </a-layout>
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

