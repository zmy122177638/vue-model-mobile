<!--
 * @Description: swiper实现多页menus组件，已有插槽可有更多拓展，只有一页时会锁定swiper，默认宫格形导航。导航请使用selectChange自定义事件
 * @Author: Anles
 * @Motto: A madman with dreams
 * @Github: https://github.com/122177638
 * @LastEditors: Anles
 * @Date: 2019-03-06 20:55:19
 * @LastEditTime: 2019-03-07 10:23:55
 -->
<template>
  <section class="navlist-container">
    <div class="swiper-container navlist-swiper-container" ref="navSwiper">
      <div class="swiper-wrapper">
        <div
          :class="[
            'swiper-slide',
            { 'swiper-no-swiping': navlist.length <= 1 }
          ]"
          v-for="(navGroup, index) in navlist"
          :key="index"
        >
          <ul class="navList-group">
            <slot v-for="(navItem, index) in navGroup" :props="navItem">
              <li
                class="navList-item"
                :key="index"
                @click="$emit('selectChange', navItem, index)"
              >
                <figure>
                  <img :src="navItem.icon" class="navitem-icon" alt="" />
                </figure>
                <p class="navitem-title">{{ navItem.title }}</p>
              </li>
            </slot>
          </ul>
        </div>
      </div>
      <!-- Add Pagination -->
      <div
        class="swiper-pagination"
        v-if="navlist.length > 1"
        ref="navSwiper_pagination"
      ></div>
    </div>
  </section>
</template>

<script>
import Swiper from "swiper";
export default {
  props: {
    // 分割数量
    slice: {
      type: Number,
      default: 8
    },
    // 数据--默认宫格形menus排版，如需更多扩展请使用slot-scope插槽
    data: {
      /**
       * @param {title} *
       * @param {icon} *
       * @Date: 2019-03-07 09:23:02
       */
      type: Array,
      required: true
    }
  },
  data() {
    return {};
  },
  computed: {
    navlist() {
      return this.sliceNavList(this.slice, this.data) || [];
    }
  },
  mounted() {
    this.$nextTick(() => {
      new Swiper(this.$refs.navSwiper, {
        centeredSlides: true,
        pagination: {
          el: this.$refs.navSwiper_pagination,
          clickable: true
        }
      });
    });
  },
  methods: {
    //分割
    sliceNavList(slideNum, totalArray) {
      let spliceNum = Math.ceil(totalArray.length / slideNum);
      let result = [];
      let startNum = 0;
      let endNum = slideNum;
      for (let i = 0; i < spliceNum; i++) {
        result.push(totalArray.slice(startNum, endNum));
        startNum += slideNum;
        endNum += slideNum;
      }
      return result;
    }
  }
};
</script>

<style lang="less">
.navlist-swiper-container {
  padding: 10px 0 25px;
  background-color: #ffffff;
  .swiper-slide {
    .navList-group {
      display: flex;
      flex-wrap: wrap;
      .navList-item {
        width: calc(100% / 4);
        flex-shrink: 0;
        display: flex;
        flex-direction: column;
        margin-top: 15px;
        .navitem-icon {
          vertical-align: middle;
          width: 44px;
          height: 44px;
          border-radius: 50%;
        }
        .navitem-title {
          font-size: 14px;
          color: #282828;
          margin-top: 3px;
        }
      }
    }
  }
  .swiper-pagination-bullet {
    width: 7px;
    height: 7px;
    border-radius: 50%;
    opacity: 1;
    background: #aaaaaa;
  }
  .swiper-pagination-bullet-active {
    width: 7px;
    height: 7px;
    border-radius: 3.5px;
    background: rgb(29, 138, 240);
  }
}
</style>
