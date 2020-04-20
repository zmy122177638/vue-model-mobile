<!--
 * @Description: 公用弹窗组件
 * @Author: Anles
 * @Motto: A madman with dreams
 * @Github: https://github.com/122177638
 * @Date: 2019-07-31 17:59:41
 * @LastEditors: Anles
 * @LastEditTime: 2019-08-01 10:48:45
 -->

<template>
  <section :class="['popup-container',{on:isShowAfter}]" v-show="isShow">
    <div class="popup-bg" @click="isMaskClose ? isShow = false : null" v-if="isMask"></div>
    <div class="popup-wrapper">
      <slot></slot>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    value: {
      type: Boolean,
      default: false
    },
    // 是否开启mask关闭
    isMaskClose: {
      type: Boolean,
      default: true
    },
    // 是否显示mask
    isMask: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      isShowAfter: this.value
    };
  },
  computed: {
    isShow: {
      get() {
        return this.value;
      },
      set(val) {
        this.$emit("input", val);
      }
    }
  },
  watch: {
    isShow(now) {
      /**
       * 为防止ui线程渲染同步导致过渡失效，异步分隔，亦可使用visibility:hidden;
       */
      setTimeout(() => {
        this.isShowAfter = now;
      }, 11);
    }
  }
};
</script>

<style lang="less" scoped>
.popup-container {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  width: 100%;
  height: 100%;
  z-index: 1000;
  .popup-bg {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    right: 0;
    opacity: 0;
    transition: opacity 0.3s ease-out 0.1s;
    background-color: rgba(0, 0, 0, 0.6);
  }
  .popup-wrapper {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%) scale(0);
    transition: all 0.3s ease-out;
  }
  &.on {
    .popup-wrapper {
      transform: translate(-50%, -50%) scale(1);
    }
    .popup-bg {
      opacity: 1;
    }
  }
}
</style>
