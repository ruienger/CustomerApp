<template>
  <div class="order">
    <van-nav-bar title="我的订单" />
    <van-tabs v-model="active" color="#1659a0">
      <van-tab title="全部">
        
      </van-tab>
      <van-tab title="未开始">
        
      </van-tab>
      <van-tab title="进行中">
        
      </van-tab> 
      <van-tab title="已完成">
        
      </van-tab>
    </van-tabs>
    <div><briup-order-item v-for="o in orders" :key="o.id" :data="o"></briup-order-item></div>
  </div>
</template>
<script>
import { mapState } from 'vuex'
import { get } from '../../http/axios'
export default {
  data(){
    return {
      active:0,
      orders: [],
      params: [
        customerId,
        status
      ]
    }
  },computed:{
    ...mapState("user",["info"])
  },created(){
    this.loadOrders()
  },methods:{
    loadOrders(){
      let url='/order/query'
      this.params.customerId = this.info.id
      get(url,this.params).then((response) => {
        this.orders = response.data
      })
    }
  }
}
</script>
<style scoped>
.order {
  background: #f1f1f1;
}
</style>