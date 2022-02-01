<template>
  <div class="app-container">
    <!-- Header头部区域 -->
    <Header></Header>

    <!-- 循环渲染每一个商品的信息 -->
    <Goods
      v-for="item in list"
      :key="item.id"
      :title="item.goods_name"
      :pic="item.goods_img"
      :price="item.goods_price"
      :state="item.goods_state"
      :id="item.id"
      @state-change="getNewState"
    >
      <!-- 使用slot插槽优化，直接父子传值，不用再兄弟之间传值了 -->
      <counter
        :num="item.goods_count"
        @addCount="changeCount(item, $event)"
        @subCount="changeCount(item, $event)"
      ></counter>
    </Goods>

    <!-- Footer区域 -->
    <Footer
      :isFull="fullState"
      @fullStateChange="changeAllState"
      :totalMoney="totalMoney"
      :totalCount="totalCount"
    ></Footer>
  </div>
</template>

<script>
// 导入axios请求库
import axios from "axios";
// 导入需要的组件
import Header from "@/components/Header/Header.vue";
import Goods from "@/components//Goods/Goods.vue";
import Footer from "@/components/Footer/Footer.vue";
import Counter from "@/components/Counter/Counter.vue";
import eventBus from "@/components/eventBus.js";

export default {
  data() {
    return {
      // 用来存储购物车的列表数据，默认为空数组
      list: [],
    };
  },
  computed: {
    // 记录商品是否都被选定上
    fullState() {
      return this.list.every((item) => item.goods_state);
    },
    // 记录商品合计价钱
    totalMoney() {
      return this.list.reduce((sum, item) => {
        if (item.goods_state) sum += item.goods_price * item.goods_count;
        return sum;
      }, 0);
    },
    // 记录商品合计数量
    totalCount() {
      return this.list.reduce((sum, item) => {
        if (item.goods_state) sum += item.goods_count;
        return sum;
      }, 0);
    },
  },
  components: {
    Header,
    Goods,
    Footer,
    Counter,
  },
  created() {
    // 调用请求数据的方法
    this.initCartList();
    // 监听商品购买数量的改变
    // eventBus.$on("share", (val) => {
    //   this.list.some((item) => {
    //     if (item.id === val.id) {
    //       item.goods_count = val.value;
    //       return true;
    //     }
    //   });
    // });
  },
  methods: {
    // 封装请求列表数据的方法
    async initCartList() {
      const { data: res } = await axios.get("https://www.escook.cn/api/cart");
      // 只要请求回来的数据，在页面渲染期间要用到，都必须转存到data中
      if (res.status === 200) {
        this.list = res.list;
      }
    },
    // 更新单个商品的选中状态
    getNewState(e) {
      this.list.some((item) => {
        if (item.id === e.id) {
          item.goods_state = e.value;
          return true;
        }
      });
    },
    // 更新所有商品的选中状态
    changeAllState(e) {
      this.list.forEach((item) => {
        item.goods_state = e.value;
      });
    },
    changeCount(item, e) {
      item.goods_count = e;
    },
  },
};
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
