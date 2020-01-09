<template>
  <div class="order_item" >
    <van-row class="header"> 
      <van-col span="12">订单编号: {{data.id}}</van-col>
      <van-col span="12" class="status">{{data.status}}</van-col>
    </van-row>
    <van-row @click="showPop">
      
      <van-col :span="24" :offset="1" >
        <div v-if="data.orderLines != null">服务：
          <span 
            v-for="line in data.orderLines" 
            :key="line.id">
              {{line.product.name}}
          </span>
        </div>
        <div v-if="data.waiter != null">员工：{{data.waiter.realname}}-{{data.waiter.telephone}}</div>
        <div>总价：{{data.total}}</div>
        <div>服务时间：{{data.orderTime | datefmt}}</div>
        <div>收货地点：{{data.address.province}}-{{data.address.city}}-{{data.address.address}}</div>
      </van-col>
    </van-row>
    <van-popup
      v-if="data.waiter === null"
      v-model="visible"
      position="bottom"
      
      :style="{ height: '40%' }"
    >
    <van-radio-group  v-for="e in employees" :key="e.id" v-model="employees.id" style="padding:1em ">
    <van-radio :name="e.id">{{e.realname}}</van-radio>
    </van-radio-group>
      <van-button block round type="primary" @click="popDownSendOrder(employees.id)" >派单</van-button> 
    </van-popup>
    <div class="text-right">
      
      共计{{data.orderLines.length}}个服务，合计￥ {{data.total}}
    </div>
  </div>
</template>
<script>
import { get } from '../http/axios'
export default {
  props:{
    data:{type:Object}
  },methods:{
    popDownSendOrder(employeeID){
      this.$emit('click',employeeID,this.data.id)
      this.visible=false
    },
    showPop(){
      const url = '/waiter/findAll'
      get(url).then((response) => {
        this.employees = response.data
      })
      // this.employees.forEach(element => {
      //   if (row.customerId === element.id) {
      //     this.shown.customerName = element.name
      //   }
      // })
      console.log("........"+this.data.id);
      this.visible = true
    } 
  },data(){
    return{
      visible:false,
      employees: []
    }
  }
}
</script>
<style scoped>
.order_item {
  margin: .5em 1em;
  padding: .5em;
  border-radius: 5px;
  background: #ffffff;
}
.order_item .header {
  line-height: 2.5em;
  font-size: 14px;
}
.order_item .header .status {
  text-align: right;
  font-size: 12px;
  color: #fd621f;
}
.order_item img {
  width: 100%;
  border-radius: 3px;
}


</style>