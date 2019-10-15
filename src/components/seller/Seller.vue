<template>
  <div class="seller">
    <Split></Split>
      <div class="address">
        <i class="iconfont ass">&#xe605;</i>
        <p>{{seller.address}}</p>
        <i class="iconfont phone">&#xe6f9;</i>
      </div>
      <Split></Split>
      <div class="serve">
        <div class="service-item">
          <i class="iconfont">&#xe611;</i>
          <p class="service">配送服务：{{seller.app_delivery_tip}}</p>
        </div>
        <div class="service-time">
            <i class="iconfont">&#xe67d;</i>
            <p class="time">配送时间：{{seller.shipping_time}}</p>
        </div>

      </div>
      <Split></Split>
      <div class="other">
        <i class="iconfont">&#xe617;</i>
        <p class="seller-service">商家服务:{{seller.app_delivery_tip}}</p>
      </div>

      <!-- <div class="news">
          <span class="icon">{{seller.discounts2[0].icon_url}}</span>
          <p class="news-item">{{seller.discounts2[0].info}}</p>    
      </div> -->
       <div class="discounts-wrapper">
           <div 
            class="discounts-item"
            v-for="(item,index) in seller.discounts2"
            :key="index"
            >
            <div class="icon">
              <img :src="item.icon_url">
            </div>
            <div class="text">
              {{item.info}}
            </div>
           </div>
        </div>
        <div class="bg"></div>
  </div>
</template>

<script>
import Split from '../split/Split'
export default {
  data(){
    return {
        seller:{}
    }
  },
  components:{
    Split
  },
  created(){
    this.axios.get("/api/seller")
    .then((res)=>{
      // console.log(res.data);
      this.seller = res.data.data;
    })
  }
  
}
</script>


<style scoped>
.seller{
  width: 100% ;
  height: 100%;
}
.seller .address{
  width: 100%;
  padding:25px 5px 30px 5px;
  background-color: #fff;
  position: relative;
  
}
.seller .address>.ass{
  width: 25px;
  font-size: 18px;
  line-height: 18px;
  float: left;
 
}
.seller .address>.phone{
  width: 30px;
  text-align: left;
  font-size: 18px;
  line-height: 18px;
  float: right;
}
.seller .address>p{
   font-size: 15px;
   line-height:15px;
   float: left;
}
.seller .serve{
  width: 100%;
  padding:25px 5px 15px 5px;
  background-color: #fff;
}
.seller .serve .service-item{
  width: 100%;
  padding-bottom: 10px;
  border-bottom: 1px solid #ccccccb0;
}
.seller .serve .service-item>i{
  width: 25px;
  font-size: 18px;
  line-height: 18px;
  float: left;
}
.seller .serve .service-item .service{
   font-size: 15px;
   line-height:15px;
   width: 100%;
}
.seller .serve .service-time{
  margin-top: 10px;
}
.seller .serve .service-time>i{
  width: 25px;
  font-size: 19px;
  line-height: 19px;
  float: left;
}
.seller .other{
  width: 100%;
  padding:25px 5px 0px 5px;
  background-color: #fff;
}
.seller .other>i{
  width: 25px;
  font-size: 18px;
  line-height: 18px;
  float: left;
}
.seller .other .seller-service{
   font-size: 15px;
   line-height:15px;
   padding-bottom: 10px;
  border-bottom: 1px solid #ccccccb0;
}
.seller .discounts-wrapper{
   width: 100%;
  padding:15px 5px 15px 5px;
  background-color: #fff;
}
.seller .discounts-wrapper .icon>img{
  width: 15px;
  height: 15px;
  float: left;
  margin-right: 10px;
}
.seller .discounts-wrapper .text{
  font-size: 15px;
   line-height:15px;
}
.seller .bg{
  background-color: #F4F4F4;
  height: 172px;
  width: 100%;
}
</style>
