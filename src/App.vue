<template>
  <div id="app">
    <VirtualScrollList :size=40 :remain=8 :items="items">
      <Item slot-scope="{item}" :item="item"></Item>
    </VirtualScrollList>
  </div>
</template>

<script>
import VirtualScrollList from './components/virtual-scroll-list'
import Item from './components/item'
let items = []
let total = 100000
for(let i=0;i<total;i++){
  items.push({id:i,value:i})
}
export default {
  name: 'App',
  components: {
    VirtualScrollList,
    Item
  },
  data(){
    return{
      items
    }
  }
}
</script>

<style lang="scss">
*{
  margin:0;
  padding: 0;
  box-sizing: border-box;//边框也会算进高度
}
</style>
