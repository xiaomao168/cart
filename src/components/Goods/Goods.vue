<template>
  <div class="goods-container">
    <!-- 左侧图片 -->
    <div class="thumb">
      <div class="custom-control custom-checkbox">
        <!-- 复选框 -->
        <input type="checkbox" class="custom-control-input" id="cb1" :checked="item.goods_state" />
        <label class="custom-control-label" for="cb1">
          <!-- 商品的缩略图 -->
          <img :src="item.goods_img" alt="图片消失" />
        </label>
      </div>
    </div>
    <!-- 右侧信息区域 -->
    <div class="goods-info">
      <!-- 商品标题 -->
      <h6 class="goods-title">{{item.goods_name}}</h6>
      <div class="goods-info-bottom">
        <!-- 商品价格 -->
        <span class="goods-price">￥{{item.goods_price}}</span>
        <!-- 商品的数量 -->
        <Counter :counter="item.goods_count"></Counter>
      </div>
    </div>
    
  </div>
</template>

<script>
//导入需要的组件
import Counter from '@/components/Counter/Counter.vue'
export default {

  props: ['cart'],
  data() {
    return {
     item:this.cart,
    }
  },

//注册组件
  components: {
    Counter,
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
