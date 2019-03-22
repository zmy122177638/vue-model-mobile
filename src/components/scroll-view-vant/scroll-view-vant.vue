<template>
  <van-pull-refresh
    class="scroll-view-container"
    v-model="isRefresh"
    ref="scrollView"
    @refresh="onRefresh"
    success-text="刷新成功"
    :head-height="55"
  >
    <template slot="pulling">
      <LogoLoading></LogoLoading>
      <p class="loading-point">下拉即可刷新...</p>
    </template>
    <template slot="loosing">
      <LogoLoading></LogoLoading>
      <p class="loading-point">释放即可刷新...</p>
    </template>
    <template slot="loading">
      <LogoLoading></LogoLoading>
      <p class="loading-point">加载中...</p>
    </template>
    <!-- vant列表数据 -->
    <van-list
      v-model="isLoading"
      :finished="isFinished"
      finished-text="没有更多了"
      @load="onLoad"
    >
      <slot>
        <p style="padding:20px" v-for="(item, index) in 40" :key="index">
          刷新次数: {{ count + index }}
        </p>
      </slot>
    </van-list>
  </van-pull-refresh>
</template>

<script>
import { List, PullRefresh } from "vant";
import LogoLoading from "@/components/logoLoading/logoLoading.vue";

export default {
  props: {
    // 是否刷新成功
    VisRefresh: {
      type: Boolean,
      default: false
    },
    // 是否加载成功
    VisLoading: {
      type: Boolean,
      default: false
    },
    // 是否加载全部
    VisFinished: {
      type: Boolean,
      default: false
    },
    // 是否加载错误
    Verror: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    isRefresh: {
      get() {
        return this.VisRefresh;
      },
      set(nowVal) {
        this.$emit("update:VisRefresh", nowVal);
      }
    },
    isLoading: {
      get() {
        return this.VisLoading;
      },
      set(nowVal) {
        this.$emit("update:VisLoading", nowVal);
      }
    },
    isFinished: {
      get() {
        return this.VisFinished;
      },
      set(nowVal) {
        this.$emit("update:VisFinished", nowVal);
      }
    },
    isError: {
      get() {
        return this.VisError;
      },
      set(nowVal) {
        this.$emit("update:VisError", nowVal);
      }
    }
  },
  components: {
    [PullRefresh.name]: PullRefresh,
    [List.name]: List,
    LogoLoading
  },
  data() {
    return {
      count: 0
    };
  },
  mounted() {},
  methods: {
    onRefresh() {
      this.$emit("onRefresh");
    },
    onLoad() {
      this.$emit("onLoad");
    }
  }
};
</script>

<style lang="less" scoped>
.scroll-view-container {
  flex: 1;
  height: 100%;
  width: 100%;
  overflow: scroll;
  -webkit-overflow-scrolling: touch;
  &::-webkit-scrollbar {
    width: 0;
    height: 0;
    color: transparent;
    background-color: transparent;
  }
  .loading-point {
    font-size: 13px;
    color: #989898;
    text-align: center;
    margin-top: 5px;
  }
  /deep/ .van-pull-refresh__head {
    line-height: normal;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
  .loading-more-box {
    height: 50px;
    line-height: 50px;
  }
}
</style>
