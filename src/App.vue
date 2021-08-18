<!--
 * @Author: Latte
 * @Date: 2021-08-18 23:28:59
 * @LAstEditors: Latte
 * @LastEditTime: 2021-08-19 01:19:15
 * @FilePath: \virtual-list\src\App.vue
-->
<template>
  <div id="app">
    <virtual-list :size="40" :remain="8" :items="items">
      <template v-slot:default="data">
        <Item :item="data.item"></Item>
      </template>
    </virtual-list>
  </div>
</template>

<script>
import virtualList from "./components/virtual-list";
import Item from "./components/item";

let items = [];
for (let i = 0; i < 10000; i++) {
  items.push({
    id: i,
    value: i,
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
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
