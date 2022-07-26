<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{counter}}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
//导入eventBus实例对象
import bus from '@/components/eventBus.js'
import { emit } from 'process'

export default {
  props: {
    //接收商品id
    id: {
      required: true,
      type:Number,
    },
    //接收商品数量
    counter: {
      default: 1,
      type:Number,
    },
 
  },

  methods: {
    sub() {
      //向App组件发送数据
      if (this.counter - 1 === 0) return;
      bus.$emit('newCount', { id: this.id, value: this.counter -1 });
    
     
    },
    add() {
      // console.log({ id:this.id,value:this.counter+1});
      //向App组件发送数据
      bus.$emit('newCount', { id: this.id, value: this.counter + 1 });
  }
 } 
}
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
