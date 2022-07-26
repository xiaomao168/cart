<template>
  <div class="app-container">
    <!-- <h1>App 根组件</h1> -->
    <Header></Header>
  
    <!-- 循环渲染每一个商品的信息 -->
    <Goods 
    v-for="item in cart" 
    :key="item.id"  
    :id="item.id"
    :state="item.goods_state"
    :img="item.goods_img"
    :title="item.goods_name"
    :price="item.goods_price"
    :count="item.goods_count"
    @stateChange="getNewState"
    ></Goods>
    <Footer :fullSelect="fullState" :totalPrice="amt" :all="total" @newFullState="getNewFull" ></Footer>
    

  </div>
</template>

<script>
//导入axios请求库
import axios from 'axios'
//导入需要的组件
import Header from '@/components/Header/Header.vue'
import Goods from '@/components/Goods/Goods.vue'
import Footer from '@/components/Footer/Footer.vue'
//导入eventBus对象
import bus from '@/components/eventBus.js'


export default {
  methods: {
    //封装请求列表数据的方法
    async initCartList() {
      //对象解构重命名
      const { data: res } = await axios.get('https://www.escook.cn/api/cart');
      if (res.status === 200) { //判断状态码
        this.cart = res.list; //数据转存到data里面
      }

    },
    //接收子组件传递过来的数据
    // e的格式为{id,value}
    getNewState(e) {
      // console.log(e);
      // console.log(this.cart);
      //调用some()方法循环查找
      this.cart.some(item => {
        if (item.id === e.id) {
          item.goods_state = e.value;
          //终止循环
          return true;
        }
      })

    },
    getNewFull(val) {
      // console.log(val);
      //根据全选状态循环所有数据
      this.cart.forEach(item => item.goods_state = val)
    },
    //接受更改后的新数据
    getNewCount(val) {
      // console.log(val);
      this.cart.some(item => {
        if (item.id === val.id) {
          
          val.value > 1 ? item.goods_count = val.value : item.goods_count = 1;
          return true;
        }
      });
    },

  },
  //生命周期函数
  created() {
    this.initCartList(); //调用方法获取数据
    //接收Counter组件发送过来的新数据
    bus.$on('newCount', (val) => {
      this.getNewCount(val);
    });
   

    
  },
  //数据
  data() {
    return {
      cart:[]
    }
  
  },
//注册组件
  components: {
    Header,
    Goods,
    Footer

  },
//计算属性
  computed: {
    //动态计算出全选状态是true还是false
    fullState() {
      //every方法循环遇到false结束循环
      return this.cart.every(item => item.goods_state);
    },
    //已勾选状态的总价格
    amt() {
      //筛选出已勾选的商品再计算总价
    return this.cart.filter(item => item.goods_state).reduce((amt, item) => {
       return  amt += item.goods_price*item.goods_count; //累加出所有商品总价 amt累加的结果
    }, 0);//0初始值
    
    },

    //已勾选商品的总数量
    total() {
      //筛选出已勾选的商品再计算总数量
     return this.cart.filter(item => item.goods_state).reduce((total, item) => {
        return total += item.goods_count;
        },0)
    }
  }
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
