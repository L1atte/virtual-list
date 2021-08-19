<!--
 * @Author: Latte
 * @Date: 2021-08-18 23:46:09
 * @LAstEditors: Latte
 * @LastEditTime: 2021-08-19 14:59:47
 * @FilePath: \virtual-list\src\components\virtual-list.vue
-->
<template>
  <div class="viewport" ref="viewport" @scroll="scrollFn">
    <!-- 滚动条 -->
    <div class="scroll-bar" ref="scrollBar"></div>
    <!-- 渲染内容 -->
    <div
      class="scroll-list"
      :style="{ transform: `translate3d(0,${offset}px,0` }"
    >
      <div
        v-for="item in visibleData"
        :vid="item.id"
        :key="item.id"
        ref="items"
      >
        <slot :item="item"></slot>
      </div>
    </div>
  </div>
</template>

<script>
import throttle from "lodash/throttle";
export default {
  props: {
    size: Number, // 当前每一项高度
    remain: Number, // 可见数量
    items: Array,
    variable: Boolean,
  },

  data() {
    return {
      start: 0, // 当前起始元素位置
      end: this.remain, // 当前末尾元素位置
      offset: 0, // 偏移量
      positions: [],
    };
  },

  created() {
    this.scrollFn = throttle(this.handleScroll, 200, { leading: false });
  },

  computed: {
    // 渲染三屏幕

    /**
     * 上一个屏幕包含的元素个数
     */
    prevCount() {
      return Math.min(this.start, this.remain);
    },

    /**
     * 下一个屏幕包含的元素个数
     */
    nextCount() {
      return Math.min(this.items.length - this.end, this.remain);
    },

    visibleData() {
      let start = this.start - this.prevCount;
      let end = this.end + this.nextCount;
      return this.items.slice(start, end); // 根据start 和 end自动取截取渲染
    },
  },

  mounted() {
    this.$refs.viewport.style.height = this.size * this.remain + "px";
    this.$refs.scrollBar.style.height = this.items.length * this.size + "px";

    // 如果加载完毕 我需要缓存每一项的高度
    this.cacheList();
  },

  // updated() {
  //   // 页面渲染完成后 需要根据当前展示的数据 更新缓存区的内容
  //   this.$nextTick(() => {
  //     // 根据当前显示的 更新缓存区的 height bottom top ，最终更新滚动条的高度
  //     let nodes = this.$refs.items // 获取真实节点
  //     if(!(nodes && nodes.length > 0)){
  //       return
  //     }
  //     nodes.forEach(node => {
  //       let {height} = node.getBoundingClientRect()
  //       let id = node.getAttribute('vid') - 0

  //       let oldHeight = this.positions[id].height
  //       let val = oldHeight - height
  //       if(val) {
  //         this.positions[id].height = height
  //         this.positions[id].bottom = this.positions[id].bottom - val
  //       }
  //     })
  //   })
  // },

  methods: {
    cacheList() {
      this.positions = this.items.map((item, index) => ({
        top: index * this.size,
        height: this.size,
        bottom: (index + 1) * this.size,
      }));
      console.log(this.positions);
    },

    getStartIndex(value) {
      let temp = null;
      let start = 0;
      let end = this.positions.length - 1;

      do {
        let middleIndex = parseInt((start + end) / 2);
        let middleValue = this.positions[middleIndex].bottom;

        if (middleValue == value) {
          return middleIndex + 1;
        } else if (middleValue < value) {
          start = middleIndex + 1;
        } else if (middleValue > value) {
          if (temp == null || temp > middleIndex) {
            temp = middleIndex; // 确定范围
          }
          end = middleIndex - 1;
        }
      } while (start <= end);

      return temp;
    },

    handleScroll() {
      console.log("inHandle");
      // 计算当前滚过去几个，当前应该从第几个开始
      let scrollTop = this.$refs.viewport.scrollTop;

      if (this.variable) {
        // 如果variable为true 说明要用二分查找找到对应的记录
        this.start = this.getStartIndex(scrollTop);
        this.end = this.start + this.remain;
        this.offset = this.positions[this.start - this.prevCount]
          ? this.positions[this.start - this.prevCount].top
          : 0;
        console.log(this.start);
      } else {
        this.start = Math.floor(scrollTop / this.size); // 获取当前应该从第几个开始渲染
        this.end = this.start + this.remain;

        // 如果有预留渲染，应该把这个位置再向上移 this.size * this.prevCount
        this.offset = this.start * this.size - this.size * this.prevCount; // 定位当前的可视区域
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.viewport {
  overflow-y: scroll;
  position: relative;
  .scroll-list {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
  }
}
</style>
