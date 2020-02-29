<template>
  <div class="goods-div">
    <!-- 左侧DIV -->
    <div class="left-div">
      <!-- 
            只要想要实现Betterscroll容器效果，必须在此DIV下套一个ul class必须是content
            固定用法就是要套ul class='content' 不要去覆盖此ul的样式！！！！！
      -->
      <ul class="content">
        <!-- 之前要滚动的内容  自己的索引和当前选中索引相同，则显示s -->
        <!-- 
            :style="{样式名: 样式值, 样式名: 样式值}"
            :class="{ 类名: 是否添加此class(true有||  false没有 ), 类2: false }"
        -->
        <div
          @click="clickLeftTitle(index)"
          :class="{leftGoods: true, selected: index == curSelected }"
          v-for="(item,index) in list"
          :key="item.name"
        >
          <!-- item.type -1没有 1折 2减 3...a  不要在v-for中使用v-if   v-if频繁操作节点，会让代码变得非常野鸡（性能低）-->
          <!-- <img style="width: 12px" v-show="item.type == 1" src="../assets/imgs/icon1.png" />
          <img style="width: 12px" v-show="item.type == 2" src="../assets/imgs/icon2.png" />-->
          <!-- <img style="width: 12px" v-show="" src="../assets/imgs/icon3.png" /> -->
          <span>{{ item.name }}</span>
        </div>
      </ul>
    </div>

    <!-- 右侧DIV -->
    <div class="right-div">
      <ul class="content">
        <div :id="index" v-for="(item,index) in list" :key="item.id">
          <!-- 标题 -->
          <h5>{{ item.name }}</h5>
          <!-- 标题下方的儿子 -->
          <div v-for="child in item.foods" :key="child.id" class="foods-child-div">
            <!-- child .ci ===  {name: "皮蛋瘦肉粥", price: 10, oldPrice: ??-->
            <img :src="child.icon" />
            <label>{{ child.name }}</label>
          </div>
        </div>
      </ul>
    </div>
  </div>
</template>

<script>
import { getGoods } from "../api/apis";
import BScroll from "better-scroll"; //引入BetterScroll滚动插件

// 怎么用api里封装的函数?
export default {
  data() {
    return {
      list: [], //食品数组
      curSelected: 0 //当前选项卡选中的索引
    };
  },
  created() {
    getGoods().then(res => {
      //设置食品数组
      this.list = res.data.data;
      console.log(res.data);
    });
  },
  mounted() {
    //new BScroll(DOM节点, 滚动配置)
    //一次只能初始化一个DIV  左右是分开的所以需要单独初始化
    //左侧滚动版
    new BScroll(document.querySelector(".left-div"), {
      click: true //允许容器进行点击
    });

    //右侧滚动板
    this.rightDiv = new BScroll(document.querySelector(".right-div"));
    // 实例对象.API函数()
  },
  methods: {
    //index:当前点击的索引
    clickLeftTitle(index) {
      //点击选项卡，把新的索引保存起来
      this.curSelected = index;

      //让右侧滚动起来 querySelector不支持数字字符串ID
      //   this.rightDiv.scrollToElement(document.querySelector('#4'))  //用实例对象.要调用的函数
      // 左侧索引index，对应右侧DIV id
      this.rightDiv.scrollToElement(document.getElementById(index), 600); //用实例对象.要调用的函数
    }
  }
};
</script>

<style lang="less" scoped>
.selected {
  background-color: #fff;
}

.goods-div {
  display: flex;
  height: 300px;

  .left-div {
    width: 80px;
    background-color: #f4f5f7;
    height: 100%;
    overflow: scroll;

    .leftGoods {
      height: 60px;
      margin: 0 8px;
      display: flex;
      align-items: center; //侧轴剧中
      border-bottom: 1px solid #e0e1e3;
    }
  }

  .right-div {
    flex: 1; //撑满父容器
    // background-color: tomato;
    overflow: scroll;

    .foods-child-div {
      img {
        width: 100px;
        height: 100px;
      }
    }
  }
}
</style>
