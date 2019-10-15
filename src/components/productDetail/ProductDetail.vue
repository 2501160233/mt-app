<template>
   	<transition name="food-detail">
    <div class="productDetail" ref="foodView"  v-show="showFlag">
      <div class="food-wrapper">
        <div class="food-content">
          <div class="img-wrapper">
            <img class="food-img" :src="selected.picture"/>
          </div>
          <div class="dele" @click="close">
            <i class="iconfont">&#xe657;</i>
          </div>
          <div class="share">
            <i class="iconfont">&#xe61f;</i>
          </div>

           <div class="content-wrapper">
              <div class="cartcontrol-wrapper">
                <p class="name">{{selected.name}}</p>
                <p class="month">{{selected.month_saled_content}}</p>
                <span class="text">￥{{selected.min_price}}</span>
                <span class="unit">/{{selected.unit}}</span>
              </div>
              <div class="checkcart">
                <div class="check" @click="addCount" v-show="!showControl">选规格</div>
                <app-cart-control :food="selected" v-show="showControl"></app-cart-control>
              </div>
            
            </div>


        </div>
    


      </div>
    </div>
  </transition>
</template>

<script>
import Vue from 'vue'
import CartControl from '../cartControl/CartControl'
export default {
  data(){
    return{
     showFlag:false,
     showControl:false
    }
  },
  props:{
       selected:{
         type:Object
       }
  
       
    },
  components:{
    "app-cart-control":CartControl
  },
  methods:{
     addCount(){
       Vue.set(this.selected,"count",1);
       this.showControl = true;
     },
      showView(){
        this.showFlag = true
      },
     close(){
        this.showFlag = false
     }
  }
  
  
}
</script>


<style scoped>
.productDetail{
  position: fixed;
	left: 0;
	top: 0;
	bottom: 51px;
	background: white;
	width: 100%;
	z-index: 90;
}
.food-detail-enter-active,.food-detail-leave-active{
  transition:all 1s;
}
.food-detail-enter,.food-detail-leave-to{
  transform: translateX(100%);
}
.productDetail .food-wrapper{
  width: 100%;
  height: 100%;
}
.productDetail .food-wrapper .food-content{
  width: 100%;
  height: 50%;
  box-sizing: border-box;
}
.productDetail .food-wrapper .food-content .img-wrapper{
  width: 100%;
  height: 100%;
}
.productDetail .food-wrapper .food-content .img-wrapper .food-img{
  width: 100%;
  height: 100%;
}
.productDetail .food-wrapper .food-content .dele{
  color:#443535c5;
  position: absolute;
  top:10px;
  left: 10px;
}
.productDetail .food-wrapper .food-content .dele>i{
  font-size: 28px;
}
.productDetail .food-wrapper .food-content .share{
  color:#443535c5;
  position: absolute;
  top:10px;
  right: 20px;
}
.productDetail .food-wrapper .food-content .share>i{
  font-size: 25px;
  font-weight: bolder;
}
.productDetail .food-wrapper .food-content .content-wrapper{
  position: relative;
  margin-top: 20px;
}
.productDetail .food-wrapper .food-content .content-wrapper .cartcontrol-wrapper .name{
  font-size: 16px;
  font-weight: bolder;
  margin:5px 0 5px 10px;
}
.productDetail .food-wrapper .food-content .content-wrapper .cartcontrol-wrapper .month{
  font-size: 14px;
  color:#30222288;
   margin:5px 0 5px 10px;
}
.productDetail .food-wrapper .food-content .content-wrapper .cartcontrol-wrapper .text{
  font-size: 16px;
  color:red;
  margin:5px 0 5px 10px;
}
.productDetail .food-wrapper .food-content .content-wrapper .cartcontrol-wrapper .unit{
   font-size: 14px;
   color:#30222288;
   margin:5px 0px 5px 0px;
}
.productDetail .food-wrapper .food-content .content-wrapper .checkcart{
  position: absolute;
  top:14px;
  right:10px;
}
.productDetail .food-wrapper .food-content .content-wrapper .checkcart .check{
   position: absolute;
    top:0px;
    right:10px;
    width: 65px;
    height: 25px;
    background-color: #ffd161;
    color:black;
    border-radius: 13px;
    text-align: center;
    font-size: 14px;
    line-height: 25px;
}
</style>
