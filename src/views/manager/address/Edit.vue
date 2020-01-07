<template>
    <briup-fulllayout title="我的地址">
    {{ form }}
    <van-cell-group>
    <van-field v-model="form.telephone" placeholder="输入手机号" />
    </van-cell-group>
    <van-cell-group>
    <van-field v-model="form.province" placeholder="输入省" />
    </van-cell-group>
    <van-cell-group>
    <van-field v-model="form.city" placeholder="输入市" />
    </van-cell-group>
    <van-cell-group>
    <van-field v-model="form.area" placeholder="输入区" />
    </van-cell-group>
    <van-cell-group>
    <van-field v-model="form.address" placeholder="输入详细地址" />
    </van-cell-group>
    <van-button block round type="primary" @click="submitHandler"/>   
    </briup-fulllayout>
</template>
<script>
import { mapState } from 'vuex'
import { post } from '../../../http/axios'
export default {
    data(){
        return{
            form:[]
        }
    },
    computed:{
        ...mapState("user",["info"])
    }
    ,methods:{
        submitHandler(){
            let url="/address/saveOrUpdate"
            this.form.customerId=this.info.id
            post(url,this.form).then((response) => {
               this.$router.go(-1)
               this.$toast.success(response,message);
           }) 
        }
    },created(){
        // this.form = this.$route.query
    }
}
</script>