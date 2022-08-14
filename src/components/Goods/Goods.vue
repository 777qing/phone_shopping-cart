<template>
  <div class="goods-container">
    <!-- 左侧图片 -->
    <div class="thumb">
      <div class="custom-control custom-checkbox">
        <!-- 复选框 -->
        <!-- 此时的选项不能用 v-model 进行双向选择因为是props传输过来的只读 -->
        <!-- 获取现在的状态通过 子向夫 传输并更改状态 -->
        <input type="checkbox" class="custom-control-input" :id="'cd'+id" :checked="state" 
        @change="stateChange"/>
        <label class="custom-control-label" :for="'cd'+id">
          <!-- 商品的缩略图 -->
          <img :src="pic" alt="" />
        </label>
      </div>
    </div>
    <!-- 右侧信息区域 -->
    <div class="goods-info">
      <!-- 商品标题 -->
      <h6 class="goods-title">{{title}}</h6>
      <div class="goods-info-bottom">
        <!-- 商品价格 -->
        <span class="goods-price">￥{{price}}</span>
        <!-- 商品的数量 -->
        <Counter :num="count" :id="id" ></Counter>
      </div>
    </div>
  </div>
</template>

<script>
import Counter from '@/components/Counter/Counter.vue'
export default {
  //APP父组件向商品传输商品信息值
  props:{
    //商品的id
    //为啥要在这里封装一个id
    //原因：将来，子组件中商品的勾选状态变化之后，
    //通过 子-父的形式，通知父组件根据id修改对应商品的勾选状态
    id:{
      require:true,
      type:Number
    },
    //商品标题
    title:{
      default:'',
      type:String
    },
    //商品图片
    //属性绑定加：{{}}用不了
    pic:{
      default:'',
      type:String
    },
    //商品价格
    price:{
      default:0,
      type:Number
    },
    //商品的选择
    state:{
      default:true,
      type:Boolean
    },
    count:{
      default:1,
      type:Number
    }
  },

  methods:{
    //只要复选框发生了变化，就会调用这个函数
    stateChange(e){
      const newState = e.target.checked
      // console.log(newState);
      //触发自定义事件
      this.$emit('state-change',{id:this.id,value:newState})
    }
  },

  components:{
    Counter
  }
}
</script>












<style lang="less" scoped>
.goods-container {
  + .goods-container {
    border-top: 1px solid #efefef;
  }
  padding: 10px;
  display: flex;
  .thumb {
    display: flex;
    align-items: center;
    img {
      width: 100px;
      height: 100px;
      margin: 0 10px;
    }
  }

  .goods-info {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
    .goods-title {
      font-weight: bold;
      font-size: 12px;
    }
    .goods-info-bottom {
      display: flex;
      justify-content: space-between;
      .goods-price {
        font-weight: bold;
        color: red;
        font-size: 13px;
      }
    }
  }
}
</style>
