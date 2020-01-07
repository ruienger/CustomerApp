<template>
    <briup-fulllayout title="我的地址">
    <center>{{ info.name }}</center>
    <!-- {{ list }}
    {{ olderList }} -->
<!-- <van-address-list
  v-model="chosenAddressId"
  :list="list"
  :disabled-list="disabledList"
  disabled-text="以下地址超出配送范围"
  default-tag-text="默认"
  @add="onAdd"
  @edit="onEdit"
/> -->
<!-- <van-cell-group>
  <van-cell title="单元格" value="内容" />
  <van-cell title="单元格" value="内容" label="描述信息" />
</van-cell-group> -->
<van-list>
  <van-cell
    v-for="item in olderList"
    :key="item"
    size="large"
    :title="item.province +'    '+ item.city +'    '+ item.area"
    :label="info.name"
  >
  <van-icon
    slot="right-icon"
    name="edit"
    style="line-height: inherit;"
    @click="toEditAddressHandler(item)"
  />
  <van-icon
    slot="right-icon"
    name="delete"
    style="line-height: inherit;"
    @click="toDeleteAddressHandler(item)"
  />
  </van-cell>
</van-list>
<br>
    <van-button block round type="primary" @click="onAdd">添加地址</van-button>  

    </briup-fulllayout>
</template>
<style lang="stylus">

</style>
<script>
import {mapState, mapActions} from 'vuex'
import { get } from '../../../http/axios';
import { url } from 'inspector';
export default {
    data() {
    return {
      // chosenAddressId: '1',
      // list: [
      //   {
      //     id: '3',
      //     name: '王五',
      //     tel: '1320000000',
      //     address: '浙江省杭州市滨江区江南大道 15 号'
      //   }
      // ],
      olderList: [
      ],
      disabledList: [
        {
          id: '3',
          name: '王五',
          tel: '1320000000',
          address: '浙江省杭州市滨江区江南大道 15 号'
        }
      ]
    }
  },computed:{
    ...mapState("user",["info"])
  },created() {
      this.loadAdress()
        // this.list = response.data
        // this.name = this.info.name
        
        // response.data.forEach(res => {
        //   console.log("res 得值: "+res)
        //   console.log(res.id,this.info.name,res.telephone,(res.province + res.city + res.area + res.address),list)
        //   this.list.push(res.id,this.info.name,res.telephone,(res.province + res.city + res.area + res.address))
        
        // });
        
        // this.list.name = 
        // this.list.tel = 
        // this.list.address = 
        
      
  },
  methods: {
    toEditAddressHandler(item){
      console.log("..............EDIT................."+item.id)
      // 跳到添加地址界面并传参
      // this.$router.push({
      //   path:"/manager/order_confirm",
      //   query:item
      // })
    },
    loadAdress(){
      let url="/address/findByCustomerId?id=" + this.info.id;
      get(url).then((response) => {
      
        this.olderList = response.data
        
        })
        // this.olderList.forEach((obj) =>{
        //   this.list.push('3','3','3','3')
        // })
    },
    onAdd() {
      this.$router.push("/manager/address_edit")
    },toDeleteAddressHandler(item){
      console.log("..............DELETE...............")
      const url="/address/deleteById"
      get(url,item.id).then((response) => {
        this.$toast("删除成功")
        this.loadAdress()
      })
    },
    onEdit(item, index) {
      Toast('编辑地址:' + index);
    }
  }
}
</script>