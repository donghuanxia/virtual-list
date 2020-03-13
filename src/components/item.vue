<template>
  <div class="item"  :vid="item.id">{{item.value}}</div>
</template>
<script>
export default {
  
  props:{
    item:Object
  }
}
</script>
<style lang="scss" scoped>
.item{
  height:40px;
  border:1px solid lightBlue;
}
</style>