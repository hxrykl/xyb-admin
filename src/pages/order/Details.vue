<template>
  <div class="orderDetails">
    <h2>订单详情</h2>
    <el-button size="small" type="text" @click="backHandler">返回</el-button>
    <el-tabs v-model="activeName">
      <el-tab-pane label="基本信息" name="info">
        <p>用户姓名：{{customer[0].realname}}</p>
        <p>服务员姓名：{{waiter[0].realname}}</p>
        <p>下单时间：{{order.orderTime}}</p>
        <p>数量：{{order.total}}</p>
        <p>状态：{{order.status}}</p>
        <p>备注：{{order.remark}}</p>
      </el-tab-pane>
      <el-tab-pane label="服务地址" name="address">
        <el-table :data="address">
          <el-table-column label="省" prop="province"></el-table-column>
          <el-table-column label="市" prop="city"></el-table-column>
          <el-table-column label="区" prop="area"></el-table-column>
          <el-table-column label="街道" prop="address"></el-table-column>
          <el-table-column label="手机号" prop="telephone"></el-table-column>
        </el-table>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>
<script>

import {mapState,mapActions} from 'vuex'
// import customer from '../../store/modules/customer';
// import address from '../../store/modules/address';

export default {
  data(){
    return {
      order:{},
      customer:{},
      waiter:[],
      activeName:"info"
    }
  },
  // vue实例化完成
  created(){
    
    // this.$route访问路由器
    this.order = this.$route.query.order;
    // 查找用户名方法
    this.findAllCustomers()
    .then((customers)=>{
      this.customer = customers.filter((item,index)=>{
        return this.order.customerId === item.id;
      })
    });
    // alert(1)
    // 查找服务员名方法
    this.findAllWaiters()
    .then((waiters)=>{
      this.waiter = waiters.filter((item,index)=>{
        return this.order.waiterId === item.id;
      })
    })
    // 通过用户Id查找地址
    this.findAddressByCustomerId(this.order.customerId)
    console.log(this.order.customerId,"gkid");
  },
  computed:{
    ...mapState("address",["address"])
  },
  methods:{
    ...mapActions("address",["findAddressByCustomerId"]),
    ...mapActions("customer",["findAllCustomers"]),
    ...mapActions("waiter",["findAllWaiters"]),
    backHandler(){
      // this.$router.push("/order")
      // 返回上一级
      this.$router.go(-1)
    }
  }

}
</script>
<style scoped>


</style>