<template>
  <div id="app-container">
    <!-- 头部区域 -->
    <my-header></my-header>
    <!-- 商品列表区域 -->
    <my-goods
      v-for="item in list"
      :key="item.id"
      :id="item.id"
      :title="item.goods_name"
      :price="item.goods_price"
      :pic="item.goods_img"
      :state="item.goods_state"
      :count="item.goods_count"
      @state-chang="getState"
    ></my-goods>
    <!-- 底部结算区域 -->
    <my-footer
      :allNum="allCount"
      :isFull="fullState"
      @newFullState="getFullState"
      :amount="amt"
    ></my-footer>
  </div>
</template>

<script>
// 导入axios请求库
import axios from "axios";
import Header from "@/components/Header/Header.vue";
import Goods from "@/components/Goods/Goods.vue";
import Footer from "@/components/Footer/Footer.vue";
import bus from "@/components/eventBus.js";

export default {
  data() {
    return {
      list: [],
    };
  },
  //计算属性
  computed: {
    // 动态计算出全选的状态是 true 还是 false
    fullState() {
      return this.list.every((item) => item.goods_state);
    },
    //已勾选商品的总价格
    amt() {
      return this.list
        .filter((item) => item.goods_state)
        .reduce((total, item) => {
          return (total += item.goods_price * item.goods_count);
        }, 0);
    },
    //已勾选商品的总数量
    allCount() {
      return this.list
        .filter((item) => item.goods_state)
        .reduce((total, item) => {
          return (total += item.goods_count);
        }, 0);
    },
  },
  components: {
    MyHeader: Header,
    MyGoods: Goods,
    MyFooter: Footer,
  },
  methods: {
    //封装请求列表数据的方法
    async initCartList() {
      const { data: res } = await axios.get("https://www.escook.cn/api/cart");
      //只要是请求回来的数据，在页面渲染其间需要使用的，则必须转存到data中
      if (res.status === 200) {
        this.list = res.list;
      }
    },
    //check单项获取选中状态
    getState(e) {
      this.list.some((item) => {
        if (item.id === e.id) {
          item.goods_state = e.value;
        }
      });
    },
    //实现footer组件中全选按钮的功能
    getFullState(e) {
      this.list.some((item) => {
        item.goods_state = e.value;
      });
    },
  },

  created() {
    //调用请求数据的方法
    this.initCartList();

    //接收counter组件通过eventBus发送来的商品数量数据
    bus.$on("shareNum", (val) => {
      this.list.some((item) => {
        if (item.id === val.id) {
          item.goods_count = val.value;
          return true;
        }
      });
    });
  },
};
</script>

<style lang="less">
#app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
