<template>
  <div class="home">
    <header class="header">
      <img src="../../assets/home.jpg" alt="">
    </header>
    <div>
      <van-grid :column-num="3">
      <van-grid-item
      v-for="value in category"
      :key="value.id"
      :icon="value.icon"
      :text="value.name"
  />
</van-grid>
<van-grid :column-num="2" icon-size="145px">
      <van-grid-item
      v-for="value in products"
      :key="value.id"
      :icon="value.photo"
      :text="value.name"
  />
</van-grid>
<briup-product-item v-for="p in products" :key="p.id" :data="p" @click="$emit('click')">
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
      category: [],
      products: []
      }
    
  },
  created(){
    this.loadCategoties()
    this.loadProducts()
  },
  methods:{
    loadCategoties(){
      let url="/category/findAll"
      get(url).then((response) => {
        // this.category = response.data.slice(0,6)
        response.data.forEach(item => {
          if(item.icon != null){
            this.category.push(item)
          }
          
        });
      })
    },
    loadProducts(){
      let url="/product/query"
      let params = {
        page:0,
        pageSize:10
      }
      post(url,params).then((response) => {
        this.products = response.data.list
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