<!--
 * @Author: Latte
 * @Date: 2021-08-18 23:46:09
 * @LAstEditors: Latte
 * @LastEditTime: 2021-08-19 01:15:41
 * @FilePath: \virtual-list\src\components\virtual-list.vue
-->
<template>
  <div class="viewport" ref="viewport">
    <!-- 滚动条 -->
    <div class="scroll-bar" ref="scrollBar"></div>
    <!-- 渲染内容 -->
    <div class="scroll-list">
      <div v-for="item in visibleData" :key="item.id">
        <slot :item="item"></slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    size: Number, // 当前每一项高度
    remain: Number, // 可见数量
    items: Array,
  },
  data() {
    return {
      start: 0,
      end: this.remain,
    };
  },
  computed: {
    visibleData() {
      return this.items.slice(this.start, this.end);
    },
  },
  mounted() {
    this.$refs.viewport.style.height = this.size * this.remain + "px";
    this.$refs.scrollBar.style.height = this.items.length * this.size + "px";
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