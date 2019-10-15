<template>
  <div class="header">
    <!-- 顶部通栏 开始 -->
    <div class="banner-top">
      <div class="arrow">
        <i class="iconfont">&#xe667;</i>
      </div>
      <div class="banner-input">
        <div class="search-icon"></div>
        <input type="text" placeholder="搜索店内商品" />
      </div>
      <div class="addition">
        <span>拼单</span>
        <i class="iconfont">&#xe66e;</i>
      </div>
    </div>
    <!--顶部通栏 结束 -->

    <!-- 主题内容 开始 -->
    <div class="logo">
      <img :src="poiInfo.pic_url" alt />
      <span>{{poiInfo.name}}</span>
    </div>
    <!-- 主题内容 结束 -->

    <!-- 公告内容 开始 -->
    <div class="notice">
      <img v-if="poiInfo.discounts2" :src="poiInfo.discounts2[0].icon_url" alt />
      <span v-if="poiInfo.discounts2" class="text">{{poiInfo.discounts2[0].info}}</span>
      <div class="detail" v-if="poiInfo.discounts2" @click="show_minute">
        {{poiInfo.discounts2.length}}个活动
        <span class="icon_arrow_right">
          <i class="iconfont">&#xe64a;</i>
        </span>
      </div>
    </div>
    <!-- 公告内容 结束 -->

    <!-- 背景 -->
    <div class="bg-wrapper" :style="head_pic_url"></div>

    <!-- 公告详情 开始 -->
    <transition name="fade">
    <div class="minute" v-if="showminute">

      <div class="main">
        <div class="minute_bg" :style="min_bg">

          <div class="minute_head" :style="min_head"></div>

          <p class="minute_name">{{poiInfo.name}}</p>

          

          <div class="minute_time">
            <span>{{poiInfo.min_price_tip}} |</span>
            <span>{{poiInfo.shipping_fee_tip}} |</span>
            <span>{{poiInfo.delivery_time_tip}} </span>
          </div>

          <div class="minute_date">配送时间：{{poiInfo.shipping_time}}</div>

          <div class="minute_btm">
             <div class="minute_btm_img" :style="min_btm"></div>
            <div class="minute_btm_text">{{poiInfo.discounts2[0].info}}</div>
          </div>
          </div>

          <!-- 关闭按钮 -->
          <div class="hidden" >
              <i class="iconfont" @click="close_minute">&#xe66f;</i>
          </div>
        </div>
      </div>
      </transition>


    </div>

    <!--公告详情 结束 -->
 
</template>

<script>
export default {
  props: {
    poiInfo: {
      type: Object,
      default: {}
    }
  },
  data() {
    return {
      showminute: false
    };
  },
  // 背景图片
  computed: {
    head_pic_url() {
      return "background-image:url(" + this.poiInfo.head_pic_url + ");";
    },
    min_bg() {
      return "background-image:url(" + this.poiInfo.poi_back_pic_url + ");";
    },
    min_head(){
      return "background-image:url("+this.poiInfo.pic_url+");";
    },
    min_btm(){
      return "background-image:url("+this.poiInfo.discounts2[0].icon_url+");";
    }
  },
  methods: {
    show_minute() {
      this.showminute = true;
    },
    close_minute(){
      this.showminute = false;
    }
  }
};
</script>


<style scoped>
/* @import url(../../common/css/iconfont.css); 在组件中引入iconfont.css */

.banner-top {
  width: 100%;
  height: 130px;
  margin-top: 20px;
  position: relative;
}
.banner-top .arrow {
  color: #ffffff;
  font-size: 20px !important;
  position: absolute;
  left: 10px;
  top: 2px;
}
.banner-top .banner-input {
  width: 50%;
  padding: 0 120px 0 40px;
}
.banner-top .banner-input input {
  width: 100%;
  height: 23px;
  outline: none;
  border-radius: 15px;
  border: 1px solid #eeeeee;
  padding-left: 30px;
  background-color: #eeeeee;
}
.banner-top .addition {
  color: #ffffff;
  /* font-size:16px; */
  position: absolute;
  right: 24px;
  top: 2px;
}
.banner-top .addition span {
  font-size: 13px;
  padding: 1px;
  border: 1px solid #ffffff;
}
.search-icon {
  width: 30px;
  height: 30px;
  background: url("./img/search.png") no-repeat 5px center;
  position: absolute;
  background-size: 16px 16px;
}

.header .bg-wrapper {
  width: 100%;
  height: 160px;
  position: absolute;
  left: 0;
  top: 0;
  z-index: -1;
  background-size: 100% 120%;
}
.logo {
  width: 90%;
  position: absolute;
  left: 10px;
  top: 77px;
  color: #ffffff;
}
.logo img {
  width: 50px;
  height: 50px;
}
.logo span {
  font-size: 16px;
  line-height: 16px;
  position: absolute;
  left: 58px;
  top: 50%;
  margin-top: -8px;
}
.notice {
  height: 15px;
  width: 100%;
  position: absolute;
  left: 10px;
  top: 140px;
}
.notice img {
  width: 16px;
  height: 16px;
  float: left;
}
.notice .text {
  font-size: 14px;
  line-height: 14px;
  color: #ffffff;
  float: left;
}
.notice .detail {
  color: #ffffff;
  font-size: 14px;
  line-height: 14px;
  float: right;
}
.notice .detail .icon_arrow_right {
  float: right;
  margin-right: 10px;
}
.minute {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.7);
  color: #ffffff;
  z-index: 999;
  display: flex;
  justify-content: center;
  align-items: center;
  
}
.minute .main {
  width: 90%;
  height: 70%;
  margin: 0 auto;
  text-align: center;
  
}
.minute .main .minute_bg {
  width: 100%;
  height: 100%;
  background-size: 100%;
  border-radius: 10px;
  padding-top:40px;
  
}
.minute .main .minute_bg .minute_head{
  width: 75px;
  height: 60px;
  background-size:100%;
  margin:auto;
}
.minute .main .minute_bg .minute_name{
  margin-top: 20px;
}
.minute .main .minute_bg .minute_time{
  margin-top: 15px;
  font-size: 10px;
  color:#eeeeee7e;
}
.minute .main .minute_bg .minute_date{
  width: 90%;
  height: 25px;
  padding-bottom: 10px;
   margin: 15px auto;
   font-size: 10px;
   color:#eeeeee7e;
   border-bottom: 1px solid #eeeeee7e;
}
.minute .main .minute_bg .minute_btm{
  width: 100%;
  height: 25px;
  margin:0 auto;
  padding-top:10px;
  display:flex;
  justify-content: center;
  
}
.minute .main .minute_bg .minute_btm .minute_btm_img{
  width: 20px;
  height: 20px;
  background-size: 100%;
  float: left;

}
.minute .main .minute_bg .minute_btm .minute_btm_text{
  font-size: 14px;
  line-height: 20px;
  float:left;
  margin-left:5px;
}
.minute .main .hidden{
  margin-top: 5px;
}
.minute .main .hidden i{
  font-size: 25px ;
}
/* 过渡动画 */
.fade-enter-active, .fade-leave-active{
   transition:opacity 1.2s;
}
.fade-enter, .fade-leave-to{
  opacity: 0;
}
.fade-leave, .fade-enter-to{
  opacity:1;
}
</style>
