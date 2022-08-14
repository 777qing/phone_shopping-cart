<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{num}}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm"  @click="add">+</button>
  </div>
</template>

<script>
import bus from '@/components/eventBus.js'

export default {
  props:{
    // 接受到的 商品的数量
    num:{
      type:Number,
      default:1
    },
    // 接受商品的id，使用 eventBus ，按数量传递到app。vue的时候
    // 需要通知App组件，更新那个商品的数量
    id:{
      type:Number,
      require:true
    }
  },


 methods:{
   add(){
     // 要发送给App的数据格式为{id,value}
     // 其中 id 是商品的 id，value 是商品最新的购物数量
     const obj = {id:this.id,value:this.num+1}
     // 要做的事情，通过 Event.bus 把obj对象，发送给App
    // console.log(obj);
    bus.$emit('share',obj)
   },

   sub(){
     if(this.num === 1){
       return
     }else{
       const obj = {id:this.id ,value:this.num-1}
     bus.$emit('share',obj)
     }
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
