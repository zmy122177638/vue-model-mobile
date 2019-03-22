<template>
  <div class="logoLoading-box">
    <img
      :src="item"
      v-for="(item, index) in imgData"
      :key="index"
      class="loading-img"
      alt=""
      v-show="index === imgIndex"
    />
  </div>
</template>

<script>
export default {
  props: {
    isloading: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      imgIndex: 0,
      countTimer: null,
      imgData: [
        require("./loading-img/loading.png"),
        require("./loading-img/loading.png"),
        require("./loading-img/loading.png"),
        require("./loading-img/loading.png"),
        require("./loading-img/loading.png"),
        require("./loading-img/loading.png"),
        require("./loading-img/loading.png"),
        require("./loading-img/loading.png"),
        require("./loading-img/loading.png")
      ]
    };
  },
  created() {
    this.countTimer = setInterval(() => {
      this.imgIndex++;
      if (this.imgIndex > 8) {
        this.imgIndex = 1;
      }
    }, 100);
  },
  destroyed() {
    clearInterval(this.countTimer);
    this.countTimer = null;
  },
  methods: {}
};
</script>

<style lang="less" scoped>
.logoLoading-box {
  text-align: center;
  .loading-img {
    width: 26px;
    height: 26px;
    vertical-align: middle;
  }
}
</style>
