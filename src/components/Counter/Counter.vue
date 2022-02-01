<template>
  <div
    class="number-container d-flex justify-content-center align-items-center"
  >
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ num }}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
import eventBus from "@/components/eventBus.js";
export default {
  props: {
    // // 商品id
    // id: {
    //   required: true,
    //   type: Number,
    // },
    // 商品购买数量
    num: {
      type: Number,
      default: 1,
    },
  },
  methods: {
    // 点击按钮，数值+1
    add() {
      // const obj = { id: this.id, value: this.num + 1 };
      // // 通过eventBus把obj对象发送给App.vue组件
      // eventBus.$emit("share", obj);
      this.$emit("addCount", this.num + 1);
    },
    // 点击按钮，数值-1
    sub() {
      if (this.num > 1) {
        // const obj = { id: this.id, value: this.num - 1 };
        // // 通过eventBus把obj对象发送给App.vue组件
        // eventBus.$emit("share", obj);
        this.$emit("subCount", this.num - 1);
      }
    },
  },
};
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
