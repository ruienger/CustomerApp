<template>
    <briup-fulllayout title="订单确认">
        <!-- {{address}} -->
        选择地址
        <van-dropdown-menu>
    <van-dropdown-item v-model="addressId" :options="options" />
    </van-dropdown-menu>
    订单详情：
    <div style="padding:0 2pm">
        <p>服务名称： {{$route.query.name}}</p>
        <p>服务价格： {{$route.query.price}}</p> 
        <p>服务数量： 1</p> 
        <p> 服务小计： {{$route.query.price}}</p>
    </div>
    <div style="position:fixed;bottom:0;width:100%;">
    <van-button round block type="primary" @click="submitHandler">提交订单</van-button>
    </div>
    </briup-fulllayout>
</template>
<script>
import { mapState } from 'vuex'
import { get,post_obj_array } from '../../../http/axios'
export default {
    data(){
        return{
            address:[],
            addressId : 0,
            orderLines : []
        }
        
    },created(){
        this.loadAdress()
        let orderLine = {
            number : 1,
            price : this.$route.query.price,
            productId : this.$route.query.id
        }
        this.orderLines.push(orderLine)
        
    },computed:{
        ...mapState("user",["info"]),
        options:function(){
            return this.address.map(item => {
                return{
                    text:item.province + " " + item.city + " "+ item.area+" "+item.address,
                    value:item.id
                }
            })
        }

    },methods:{
        submitHandler(){
            let url = "/order/save"
            let data = {
                customerId:this.info.id,
                addressId: this.addressId,
                orderLines: this.orderLines
            }
            post_obj_array(url,data).then((response) =>{
                this.$toast("提交成功")
                this.$router.push("/manager/order")
            })
        },
        loadAdress(){
      let url="/address/findByCustomerId?id=" + this.info.id;
      get(url).then((response) => {
      
        this.address = response.data
        console.log("addressId[0]..................."+this.address[0].id);
        this.addressId = this.address[0].id
        console.log("addressId......."+this.addressId);
        })
        // this.olderList.forEach((obj) =>{
        //   this.list.push('3','3','3','3')
        // })
    }
    }
}
</script>
<style scoped>
</style>