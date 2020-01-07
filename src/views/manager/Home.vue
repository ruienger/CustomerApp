<template>
  <div class="home">
    <header class="header">
      <img src="../../assets/home.jpg" alt="">
    </header>
    <van-cell title="栏目" is-link :arrow-direction="direction" value="更多" @click="showMoreCategory"/>
    <div>
      <van-grid :column-num="3">
      <van-grid-item
      v-for="value in category"
      :key="value.id"
      :icon="value.icon"
      :text="value.name"
  />
</van-grid>
<van-cell title="产品" is-link :arrow-direction="direction" value="更多" @click="showMoreProduct"/>
<van-grid :column-num="2" icon-size="145px">
      <van-grid-item
      v-for="value in products"
      :key="value.id"
      :icon="value.photo"
      :text="value.name"
  />
</van-grid>
<briup-product-item v-for="p in products" :key="p.id" :data="p" @click="toBuyItemHandler(p)">
</briup-product-item>
    </div>
  </div>
</template>
<script>
import {mapState, mapActions} from 'vuex'
import { get,post } from '../../http/axios'
export default {
  data() {
    return{
      direction: 'down',
      category: [],
      products: []
      }
    
  },
  created(){
    this.loadProducts(true)
    this.loadCategoties(true)
  },
  methods:{
    showMoreCategory(){
      if(this.direction === 'down'){
      this.direction = 'up'
      this.loadCategoties(false)
      
      }else{
        this.direction = 'down'
        this.loadCategoties(true)
      }
    },
    showMoreProduct(){
      console.log(".........................");
      if(this.direction === 'down'){
      this.direction = 'up'
      this.loadProducts(false)
      
      }else{
        this.direction = 'down'
        this.loadProducts(true)
      }

    },
    loadCategoties(isShort){
      let url="/category/findAll"
      get(url).then((response) => {
        // this.category = response.data.slice(0,6)
        this.category = []
        if(isShort){
          response.data.forEach(item => {
          if(item.icon != null){
            this.category.push(item)
          }
          
        });
        }else{this.category = response.data}
        
        
        
      })
    },
    loadProducts(isShort){
      let url="/product/query"
      let params = {
        page:0,
        pageSize:10
      }
      this.products = []
      post(url,params).then((response) => {
        if(isShort){
          this.products = response.data.list.slice(0,4)
        
        }else{
          this.products = response.data.list
        }
        
      })
    },toBuyItemHandler(product){
      this.$router.push({
        path:"/manager/order_confirm",
        query:product
      })
    }
  }
}
</script>
<style scoped>
.home {
  padding-bottom: 50px;
}
.header {
  height: 300px;
  overflow: hidden;
}
.header img {
  width: 100%;
}
</style>