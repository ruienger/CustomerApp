<template>
  <div class="order">
    <van-nav-bar title="我的订单" />
    <van-tabs v-model="active" color="#1659a0" @click="onClick">
      <van-tab title="全部">
        
      </van-tab>
      <van-tab title="待派单">
        
      </van-tab>
      <van-tab title="待接单">
        
      </van-tab> 
      <van-tab title="待服务">
        
      </van-tab>
      <van-tab title="待确认">
        
      </van-tab>
      <van-tab title="已完成">
        
      </van-tab>
    </van-tabs>
    <!-- @click="loadOrders" -->
    <div><briup-order-item v-for="o in orders" :key="o.id" :data="o" @click="showPopAndSendHandler(arguments)" @confirm="confirmOrder(arguments)"></briup-order-item></div>
    
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
      params: {
        status : null
      }
    }
  },computed:{
    ...mapState("user",["info"])
  },created(){
    this.loadOrders()
  },methods:{
    confirmOrder(arg){
      let url = '/order/confirmOrder?orderId='+arg[0]
      console.log('........' + arg[0])
      get(url).then((response) => { 
        this.$toast("确认成功")
      })
      this.visible = false
      this.loadOrders()
    },
    showPopAndSendHandler(msg){
      let url = '/order/sendOrder?waiterId='+msg[0]+'&orderId='+msg[1]
      console.log('........' + msg[0] + msg[1])
      get(url).then((response) => { 
        this.$toast("派单成功")
      })
      this.visible = false
      this.loadOrders()
    },
    loadOrders(){
      let url='/order/query'
      this.params.customerId = this.info.id
      get(url,this.params).then((response) => {
        this.orders = response.data
      })
    },onClick(name,title){
      console.log("..........................................."+title+"....."+name);
      // switch(this.active){
      //   case 0:
      //     this.params.status = null
      //     console.log("....."+this.params.status);
      //     this.loadOrders()
      //     break
      //   default:
      //     this.params.status = title
      //     console.log("....."+this.params.status);
      //     this.loadOrders()

      //     break

      // }
      this.params.status = title === "全部"? null: title
      console.log("....."+this.params.status)
      this.loadOrders()
    }
  }
}
</script>
<style scoped>
.order {
  background: #f1f1f1;
}
</style>