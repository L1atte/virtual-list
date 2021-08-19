<!--
 * @Author: Latte
 * @Date: 2021-08-18 23:28:59
 * @LAstEditors: Latte
 * @LastEditTime: 2021-08-19 15:07:22
 * @FilePath: \virtual-list\src\App.vue
-->
<template>
  <div id="app">
    <!-- size表示元素统一高度， remain表示每一页显示多少元素， variable表示元素高度不统一 -->
    <virtual-list :size="80" :remain="10" :items="items" :variable="true">
      <template v-slot:default="data">
        <Item :item="data.item"></Item>
      </template>
    </virtual-list>
  </div>
</template>

<script>
import virtualList from "./components/virtual-list";
import Item from "./components/item";
import Mock from "mockjs";

let items = [];
for (let i = 0; i < 100000; i++) {
  items.push({
    id: i,
    value: Mock.Random.sentence(5, 50),
  });
}

export default {
  name: "App",
  components: {
    virtualList,
    Item,
  },
  data() {
    return {
      items,
    };
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
