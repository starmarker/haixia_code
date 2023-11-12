<template>
  <div id="background">
    <div class="container">
      <form action="">
        <h1>登录</h1>
        <div class="form">
          <div class="item">
            <label>用户名：</label><input type="text" name="username" v-model="data.username" placeholder="请输入用户名">
            <!-- v-model把输入的值传输给name变量 -->
            <br />
          </div>
          <div class="item">
            <label>密码：</label><input type="password" name="password" v-model="data.password" placeholder="请输入密码">
            <br />
          </div>
        </div>

      </form>
      <button type="submit" @click="onlogin">登录</button>
      <!-- v-on点击按钮触发handlelogin方法 -->
      <router-view></router-view>
    </div>
  </div>
</template>

<script setup>
import { reactive } from 'vue'
import { useStorage } from '@vueuse/core';
import {useRouter} from 'vue-router'
import {USER_INFO_KEY} from '@/config/config'
const data = reactive({
  username: '',
  password: '',
  loginlevel: 1 // //0为普通权限，1为高级权限
})
const $userInfo = useStorage(USER_INFO_KEY,{},sessionStorage);
const router = useRouter()
function onlogin() {
  // if(this.name===localStorage['name']&&this.password===localStorage['password'])
  if (data.username != "" && data.password === "123") {////如果输入的名字以及密码正确路由跳转至首页
    $userInfo.value = data;
    router.push("/");
  }
  else { return false; }
}


</script>

<style scoped>
#background {
  width: 100%;
  height: 100%;
  background: #525bab;
  background-size: 100% 100%;
  position: fixed;
  top: 0;
  left: 0;
}

.container {
  width: 480px;
  height: 300px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: #00000090;
  text-align: center;
  border-radius: 20px;
  margin-top: 10px;
}

.container h1 {
  color: aliceblue;
  margin-left: 20px;
}

.item {
  color: white;
  margin-left: 15%;
  margin-top: 35px;
  font-size: 20px;
  text-align: left;
}

.item label {
  float: left;
  width: 5em;
  margin-right: 1em;
  text-align: right;
}

input {
  margin-left: -5px;
  padding: 4px;
  border: solid 1px #4e5ef3;
  outline: 0;
  font: normal 13px/100% Verdana, Tahoma, sans-serif;
  width: 200px;
  height: 23px;
  background: #f1f1f190;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 0px 8px;
}

button {
  position: relative;
  height: 33px;
  width: 100px;
  background: rgba(35, 19, 252, 0.425);
  border-radius: 10px;
  margin-top: 18px;
  box-shadow: none;
  color: white;
  margin-left: 40px;
  margin-right: 10px;

}
</style>
    