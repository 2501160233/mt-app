<template>
    <div class="shopcart">
    
         <!-- 底部左侧 -->
        <div class="content-left" >
            <div class="logo" :class="{'highlight':totalCount>0}">
                <i 
                    class="iconfont logo-car"  
                    :class="{'highlight':totalCount>0}"
                    @click="toggleList">
                    &#xe62d;
                </i>
                <transition name="movecount">
                    <i  
                        class="totalCount" 
                        v-show="totalCount">
                        {{totalCount}}
                    </i>
                </transition>
            </div>
            <i class="totalPrice" v-show="totalPrice">￥{{totalPrice}}</i>
            <span 
                class="desc"
                :class="{'highlight':totalCount>0}">
                另需配送费{{poiInfo.shipping_fee_tip}}
            </span>
        </div>
          <!-- 底部右侧 -->
        <div 
            class="content-right"
            :class="{'highlight':totalCount>0}">
            <!-- {{poiInfo.min_price_tip}}元 -->
            {{payStr}}
        </div>

        <!-- 购物车列表 -->
         <div class="shopcart-wrapper" >
        <div class="shopcart-list"  v-show="listShow"  :class="{'show':listShow}">
            <div 
                class="list-top" 
                v-if="poiInfo.discounts2">
                {{poiInfo.discounts2[0].info}}
            </div>
            <div class="list-header">
                <h3 class="title">1号口袋</h3>
				<div class="empty" >
					<img src="./img/ash_bin.png" />
					<span @click="clearAll">清空购物车</span>
				</div>
            </div>
            <div class="list-content" ref="listContent">
				<ul>
					<li 
						class="food-item"
						v-for="(food,index) in watchFoodCount"
						:key="index"
						>
						<div class="desc-wrapper">
							<div class="desc-left">
								<p class="name">{{food.name}}</p>
							</div>
							<div class="desc-right">
								￥{{food.min_price}}
							</div>
						</div>
						<div class="cartcontrol-wrapper">
							<app-cart-control :food="food"></app-cart-control>
						</div>
					</li>
				</ul>
			</div>
            <div class="list-bottom"></div>
        </div>
        </div>
        <!-- 蒙版 -->
        <div class="shopcart-mask" v-show="listShow" @click="hideMask"></div>
    </div>
   
</template>

<script>
import BScroll from 'better-scroll'
import CartControl from '../cartControl/CartControl'
export default {
    props:{
        poiInfo:{
            type:Object,
            default:{}
        },
       watchFoodCount:{
           type:Array,
            default(){
                return []
            }
       }
    },
    data(){
        return {
            // 点击购物车
            cartToggle:true
        }
    },
    components:{
        'app-cart-control':CartControl
    },
    computed:{
        // 计算购物车内商品数目
        totalCount(){
            let num = 0;
            this.watchFoodCount.forEach((food)=>{
                num += food.count;
            })
            return num;
        },
        totalPrice(){
            let price = 0;
            this.watchFoodCount.forEach((food)=>{
                price += food.min_price * food.count;
            })
            return price;
        },
        payStr(){
            if(this.totalCount>0){
                return "去结算"
            }
            else{
                return this.poiInfo.min_price_tip;
            }
        },
        
        listShow(){
            // 购物车数目为0
            if(!this.totalCount){
                this.cartToggle = true;
                return false;
            }
            let show = !this.cartToggle;

            if(show){
                // 页面重新渲染时调用
                this.$nextTick(() => {
						if(!this.shopScroll){
                            // 实例化滚动
							this.shopScroll = new BScroll(this.$refs.listContent,{
								click:true
							})
						}else{
                            // 当页面发生变化时刷新
							this.shopScroll.refresh()
						}
					})
            }
            return show;

        }
    },
    methods:{
        toggleList(){
            // 判断购物车内有无内容
            if(this.totalCount == 0){
                return ;
            }
            this.cartToggle = !this.cartToggle;
        },
        clearAll(){
            this.watchFoodCount.forEach((food)=>{
                food.count = 0;
            })
        },
        hideMask(){
            this.cartToggle = true;
        }
    }
    
  
}
</script>


<style scoped>
.shopcart{
    height: 55px;
    width: 100%;
	background: #514f4f;
    position: fixed;
	left: 0;
	bottom: 0px;
	display: flex;
	z-index: 99;

}
.shopcart .content-left{
    flex:1;
    position: relative;
}
.shopcart .content-right{
  	flex: 0 0 110px;
    font-size: 15px;
	color: #BAB9B9;
	line-height: 51px;
	text-align: center;
	margin-right: 0px !important;
}
.shopcart .content-right.highlight{
    background-color: orange;
    color:black;
   
}
.shopcart .content-left .logo{
    width: 50px;
    height: 50px;
    margin-top: -14px;
    margin-left: 10px;
    border-radius: 50%;
    background: #666666;
    position: relative;
    text-align: center;
}

.shopcart .content-left .logo .logo-car{
   	font-size: 28px;
	color: #c4c4c4;
	line-height: 50px;
}
.shopcart .content-left .logo.highlight{
    background: orange;
}
.shopcart .content-left .logo .logo-car.highlight{
    color:#000;
}
.shopcart .content-left .desc{
    font-size: 12px;
    line-height: 51px;
    color: #bab9b9;
    position: absolute;
    left: 75px;
    top: 50%;
    transform: translateY(-50%);
}
.shopcart .content-left .desc.highlight{
   top:65%;
}
.shopcart .content-right{
    font-size: 12px;
    line-height: 51px;
    color: #bab9b9;
    float: right;
    margin-right: 20px;
}
.shopcart .content-left .logo .totalCount{
    width: 18px;
    height: 18px;
    font-size: 15px;
    border-radius: 50% ;
    background-color:red;
    color:#ffffff;
    position: absolute;
    top:0px;
    right:-3px;
}
.shopcart .content-left .totalPrice{
    position: absolute;
    left: 75px;
    top:5px;
    font-size: 18px;
    color:#ffffff;
}

/* 购物车列表样式 */

.shopcart-wrapper .shopcart-list{
	position: absolute;
	left: 0;
	top: 0;
	z-index: -1;
	width: 100%;
    
}
.shopcart-wrapper .shopcart-list.show{
	transform: translateY(-100%);
}

.shopcart-wrapper .shopcart-list .list-top{
	height: 30px;
	text-align: center;
	font-size: 11px;
	background: #f3e6c6;
	line-height: 30px;
	color: #646158;
}

.shopcart-wrapper .shopcart-list .list-header{
	height: 30px;
	background: #F4F4F4;
}
.shopcart-wrapper .shopcart-list .list-header .title{
	float: left;
	border-left: 4px solid #53c123;
	padding-left: 6px;
	line-height: 30px;
	font-size: 12px;
}
.shopcart-wrapper .shopcart-list .list-header .empty{
	float: right;
	line-height: 30px;
	margin-right: 10px;
	font-size: 0;
}
.shopcart-wrapper .shopcart-list .list-header .empty img{
	height: 14px;
	margin-right: 9px;
	vertical-align: middle;
}
.shopcart-wrapper .shopcart-list .list-header .empty span{
	font-size: 12px;
	vertical-align: middle;
}

.shopcart-wrapper .shopcart-list .list-content{
	max-height: 360px;
	overflow: hidden;
	background: white;
}
.shopcart-wrapper .shopcart-list .list-content .food-item{
	height: 38px;
	padding: 12px 12px 10px 12px;
	border-bottom: 1px solid #F4F4F4;
}
.shopcart-wrapper .shopcart-list .list-content .food-item .desc-wrapper{
	float: left;
	width: 240px;
}
.shopcart-wrapper .shopcart-list .list-content .food-item .desc-wrapper .desc-left{
	float: left;
	width: 170px;
}
.shopcart-wrapper .shopcart-list .list-content .food-item .desc-wrapper .desc-left .name{
	font-size: 16px;
	margin-bottom: 8px;
	/* 超出部分隐藏*/
	-webkit-line-clamp: 1;
	display: -webkit-box;
	-webkit-box-orient: vertical;
	overflow: hidden;
	line-height: 38px;
}
.shopcart-wrapper .shopcart-list .list-content .food-item .desc-wrapper .desc-left .unit{
	font-size: 12px;
	color: #B4B4B4;
}
.shopcart-wrapper .shopcart-list .list-content .food-item .desc-wrapper .desc-left .description{
	font-size: 12px;
	color: #B4B4B4;
		
	/* 超出部分隐藏*/
	overflow: hidden;
	line-height: 38px;
}
.shopcart-wrapper .shopcart-list .list-content .food-item .desc-wrapper .desc-right{
	float: right;
	width: 70px;
	text-align: right;	
    line-height: 38px;
}
.shopcart-wrapper .shopcart-list .list-content .food-item .desc-wrapper .desc-right .price{
	font-size: 12px;
	line-height: 38px;
}

.shopcart-wrapper .shopcart-list .list-content .food-item .cartcontrol-wrapper{
	float: right;
	margin-top: 6px;
}

.shopcart .shopcart-mask{
	position: fixed;
	top: 0;
	right: 0;
	width: 100%;
	height: 100%;
	z-index: -10;
	background: rgba(7,17,27,0.6);
}

</style>
