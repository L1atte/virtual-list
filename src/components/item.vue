<!--
 * @Author: Latte
 * @Date: 2021-08-19 00:21:52
 * @LAstEditors: Latte
 * @LastEditTime: 2021-08-19 01:18:46
 * @FilePath: \virtual-list\src\components\item.vue
-->
<template>
  <div style="height: 40px; border:1px solid red">
    {{item.value}}
  </div>
</template>

<script>

export default ({
  props: {
    item: Object,
  },
  created() {
    console.log("312312",this.item)
  }
}) 
</script>
