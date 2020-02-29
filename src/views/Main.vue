<template>
  <div>
    <!-- 容器页面 -->
    <div class="head-div" :style="{backgroundImage: 'url(' + data.avatar + ')'}">
      <!-- 联调拿到后台数据？ -->
      <img :src="data.avatar" />
      <div>
        <h2>{{ data.name }}</h2>
        <p>{{data.description}}</p>
      </div>
    </div>

    <div class="router-link-div">
      <router-link to="/goods">商品</router-link>
      <router-link to="/evaluate">评价</router-link>
      <router-link to="/merchant">商家</router-link>
    </div>
    <!-- 二级路由出口 -->
    <router-view></router-view>

    <div class="shopcar-bar">购物车条</div>
    
  </div>
</template>

<script>
import { getSeller } from "../api/apis";
export default {
  data() {
    return {
      data: {} //商家信息
    };
  },
  created() {
    //发送初始化请求
    //   var obj =  getSeller() // req.get('/api/seller')
    //     // obj === req.get('/api/seller')
    //     obj.then()

    getSeller().then(res => {
      // 怎么把商家数据渲染到屏幕上？？？
    //   console.log(res.data.data);
      this.data = res.data.data;
    });
  }
};
</script>

<style lang="less" scoped>
.head-div {
  padding: 12px;
  display: flex;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
//   filter: blur(1px);

  img {
    width: 120px;
    height: 120px;
  }
}

.router-link-div {
    margin: 8px;
  display: flex;
  justify-content: space-around;
}

.shopcar-bar {
  position: fixed; //脱离文档流-相对于浏览器绝对定位
  height: 60px;
  width: 100%;
  bottom: 0; //永远挨着浏览器底边
  background-color: #141c27;
}
</style>