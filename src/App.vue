<template>
  <div class="app-container">
    <!-- <h1>App 根组件</h1> -->
    <Header></Header>
    <!-- 循环渲染 -->
    <Goods v-for="item in cart" 
    :key="item.id"  
    :state="item.goods_state"
    :img="item.goods_img"
    :title="item.goods_name"
    :price="item.goods_price"
    :count="item.goods_count">
    </Goods>
    <Footer></Footer>
    

  </div>
</template>

<script>
//导入axios请求库
import axios from 'axios'
//导入需要的组件
import Header from '@/components/Header/Header.vue'
import Goods from '@/components/Goods/Goods.vue'
import Footer from '@/components/Footer/Footer.vue'


export default {
  methods: {
    //封装请求列表数据的方法
    async initCartList() {
      //对象解构重命名
      const { data: res } = await axios.get('https://www.escook.cn/api/cart');
      if (res.status === 200) { //判断状态码
             this.cart = res.list; //数据转存到data里面
      }
 
    }
    
  },
  created() {
   this.initCartList(); //调用方法获取数据


    
  },
  data() {
    return {
      cart:[]
    }
  
},
  components: {
    Header,
    Goods,
    Footer

  }
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
