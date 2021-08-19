<!--
 * @Author: Latte
 * @Date: 2021-08-19 00:21:52
 * @LAstEditors: Latte
 * @LastEditTime: 2021-08-19 14:02:12
 * @FilePath: \virtual-list\src\components\item.vue
-->
<template>
  <div style="border: 1px solid red; padding: 20px 0">
    {{ item.id }} {{ item.value }}
  </div>
</template>

<script>
export default {
  props: {
    item: Object,
  },
};
</script>
