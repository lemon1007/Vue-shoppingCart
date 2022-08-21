<template>
  <div class="footer-container">
    <!-- 左侧的全选 -->
    <div class="custom-control custom-checkbox">
      <input
        type="checkbox"
        class="custom-control-input"
        id="cbFull"
        :checked="isFull"
        @change="fullStateChang"
      />
      <label class="custom-control-label" for="cbFull">全选</label>
    </div>

    <!-- 中间的合计 -->
    <div>
      <span>合计：</span>
      <span class="total-price">￥{{ amount.toFixed(2) }}</span>
    </div>

    <!-- 结算按钮 -->
    <button type="button" class="btn btn-primary btn-settle">
      结算（{{ allNum }}）
    </button>
  </div>
</template>

<script>
export default {
  props: {
    isFull: {
      type: Boolean,
      default: true,
    },
    amount: {
      type: Number,
      default: 0,
    },
    allNum: {
      type: Number,
      default: 0,
    },
  },
  methods: {
    fullStateChang(e) {
      const efullState = e.target.checked;
      this.$emit("newFullState", { value: efullState });
    },
  },
};
</script>

<style lang='less' scoped>
.footer-container {
  color: white;
  font-size: 12px;
  height: 50px;
  width: 100%;
  position: fixed;
  bottom: 0;
  left: 0;
  background-color: orange;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 10px;
  z-index: 999;
}

.custom-checkbox {
  display: flex;
  align-items: center;
}

#cbFull {
  margin-right: 5px;
}

.btn-settle {
  height: 65%;
  min-width: 110px;
  border-radius: 25px;
  font-size: 12px;
  margin-right: 5%;
  border: none;
}

.total-price {
  font-weight: bold;
  font-size: 14px;
  color: red;
}
</style>