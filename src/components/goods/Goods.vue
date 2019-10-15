<template>
  <div class="goods">
      <!-- 分类列表 -->
      <div class="menu-wrapper" ref="menuScroll">
        <ul>
          <!-- 专场 -->
        <li 
            class="menu-item" 
            :class="{'current':currentIndex ===0}"
            @click="selectMenu(0)"
        >
          <p class="text">
          <img class="icon" :src="container.tag_icon" v-if="container.tag_icon">
          {{container.tag_name}}
          </p>

        </li>

        <li 
            class="menu-item" 
            :class="{'current':currentIndex ===index+1}" 
            v-for="(item,index) in goods" :key="index"
            @click="selectMenu(index+1)"
        >
          <p class="text">
          <img class="icon" :src="item.icon" v-if="item.icon">
          {{item.name}}
          </p>

        </li>
        
        </ul>
      </div>
      <!-- 商品列表 -->
      <div class="foods-wrapper" ref="foodScroll">
        <ul>
          <!-- 专场 -->
          <li class="container-list food-list-hook">
            
              <div v-for="(item,index) in container.operation_source_list" :key="index">
                <img class="list-img" :src="item.pic_url" alt="">
              </div>

          </li>
          <!-- 具体分类 -->
          <li class="food-list food-list-hook"  v-for="(item,index) in goods" :key="index">
                <h3 class="title">{{item.name}}</h3>
               <!-- 具体商品分类 -->
               <ul>
                 <li 
                    class="food-item" 
                    v-for="(i,index) in item.spus" 
                    :key="index"
                    @click="showProductDetail(i)">
                   <div class="icon" :style="food_icon(i.picture)"></div>
                   
                   <div class="food-content">
                      <h3 class="name">{{i.name}}</h3>
                      <p class="desc" v-if="i.description">{{i.description}}</p>
                      <div class="extra">
                        <span class="saled">月销{{i.month_saled}}</span>
                        <span class="praise">{{i.praise_content}}</span>
                      </div>
                      <p class="price">
                        <span class="text">${{i.min_price}}</span>
                        <span class="unit">/{{i.unit}}</span>
                      </p>
                   </div>
                    
                    <!-- 加号 -->
                    <div class="cartControl-wrapper">
                      <app-cart-control :food="i"></app-cart-control>
                    </div>

                 </li>
               </ul>
              
          </li>
        </ul>
      </div>
      <!-- 购物车 -->
      <app-shopcart :poiInfo="poiInfo" :watchFoodCount="watchFoodCount"></app-shopcart>
      <!-- 商品详情 -->
      <app-product-detail  :selected="selected" ref="foodView"></app-product-detail>

  </div>
</template>

<script>
import BScrool from 'better-scroll'
import Shopcart from '../shopcart/Shopcart'
import CartControl from '../cartControl/CartControl'
import ProductDetail from '../productDetail/ProductDetail.vue'
export default {
  data(){
    return{
      container:"",
      goods:[],
      listHeight:[],
      poiInfo:{},
      menuScroll:"",
      foodScroll:"",
      scrollY:0,
      selected:{},
      
    }
  },
  components:{
      "app-shopcart":Shopcart,
      "app-cart-control":CartControl,
      "app-product-detail":ProductDetail
  },

  // 不能使用computed,他不能传参
  methods:{
    food_icon(imgName){
      return "background-image:url("+imgName+");"
    },
    initScroll(){
      this.menuScroll = new BScrool(this.$refs.menuScroll,{
        click:true //拥有点击事件
      });
      this.foodScroll = new BScrool(this.$refs.foodScroll,{
        click:true,
        probeType:3,
      });

       // foodScroll监听事件
      this.foodScroll.on("scroll",(pos)=>{
       
        this.scrollY = Math.abs(Math.round(pos.y));
        // console.log(this.scrollY)
      })

    },
    // 计算高度
    calculateHeight(){
      // 获取元素
        var foodlist = this.$refs.foodScroll.getElementsByClassName("food-list-hook");
        // console.log(foodlist)
        let height = 0;
        this.listHeight.push(height);
       
        for(let i=0; i<foodlist.length; i++){
          let item = foodlist[i];
          height += item.clientHeight;
           
          this.listHeight.push(height);
         
        }
        // console.log(this.listHeight)
    },
    selectMenu(index){
      // console.log(index);
      var foodlist = this.$refs.foodScroll.getElementsByClassName("food-list-hook");
      let element = foodlist[index]
      // console.log(element)
      // 滚动到对应元素的位置
      this.foodScroll.scrollToElement(element,250)
    },

     showProductDetail(food){
      
      this.selected = food;

    //  找到子组件中的foodView，调用showView方法
    
      this.$refs.foodView.showView()
      
    }

  },

  created(){
    // axios
    this.axios.get("/api/goods")
    .then(res=>{
        this.container = res.data.data.container_operation_source;
        this.goods = res.data.data.food_spu_tags;
        this.poiInfo = res.data.data.poi_info;
        // console.log(this.poiInfo);

       // 要在Created里操作dom需要用nextTick回调，因为created执行时DOM还未创建
          this.$nextTick(() => {
            // 执行滚动方法
            this.initScroll()

            // 计算分类的区间高度
            this.calculateHeight()
            // 监听滚动的位置
            // 根据滚动位置确认下标,与左侧对应
            // 通过下标实现点击左侧,滚动右侧
          })
    })

  },
  computed:{
    currentIndex(){
     
      for(let i=0;i<this.listHeight.length;i++){
           // 获取商品区间的范围
        let height1 = this.listHeight[i];
        let height2 = this.listHeight[i+1];
          // 是否在上述区间中
        if(!height2 || this.scrollY >= height1 && this.scrollY < height2){
           return i;
        }
      }
      return 0;
    },

    // 添加商品时监听count有无变化,并返回商品数组
    watchFoodCount(){
      // 定义我的商品数组
      let myfoods = [];
      this.goods.forEach(element => {
        element.spus.forEach(food=>{
          if(food.count > 0){
            myfoods.push(food);
          }
        })
      });

      return myfoods;
    }
  }

  
}
</script>


<style scoped>
.goods{
  display: flex;
  position: absolute;
  top: 190px;
  bottom:41px;
  width: 100%;
  overflow: hidden;
}
.goods .menu-wrapper{
  /* 主轴方向宽度保持80不变 ，不会收缩 拉伸*/
  flex:0 0 75px;
  background-color: #f4f4f4;
}
.goods .menu-wrapper .menu-item{

  padding:13px 0px 12px 10px;
  border-bottom:1px solid #e4e4e4;

  
}
.goods .menu-wrapper .menu-item .text{
  font-size: 14px;
  color:#333333;
  
  line-height: 15px;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
 	-webkit-box-orient: vertical;
  
}
.goods .menu-wrapper .menu-item .text .icon{
  width: 15px;
  height: 15px;
  vertical-align: middle;
}
.goods .foods-wrapper{
  flex:1;
}
.goods .foods-wrapper .container-list{
  padding: 10px;
  border-bottom: 1px solid #e4e4e4;
}
.goods .foods-wrapper .container-list .list-img{
  border-radius: 5px;
  width: 100%;
  margin-bottom: 10px;
}
.goods .foods-wrapper .food-list .title{
    font-size: 13px;
    height: 13px;
    background:url(./img/btn_yellow_highlighted@2x.png) no-repeat left center;
    background-size: 2px 10px;
    padding-left:7px;
    margin-bottom:12px;
    font-weight: bolder;
    margin-top: 10px;
}

.goods .foods-wrapper .food-list .food-item{
  width: 100%;
  display: flex;
  margin-bottom: 25px;
  padding:0 10px 0 10px;
}
.goods .foods-wrapper .food-list .food-item .food-content{
  flex:1;
}
.goods .foods-wrapper .food-list .food-item .food-content .desc{
    color:#1d0c0c93;
    font-size: 14px;
    margin-bottom: 7px;
    line-height: 15px;
    -webkit-line-clamp:1;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    overflow: hidden;
    text-overflow: ellipsis;
}
.goods .foods-wrapper .food-list .food-item .icon{
  flex:0 0 63px;
  background-size: 120% 100%;
  background-position: center;
  background-repeat: no-repeat;
	margin-right: 11px;
	/* height: 75px; */
}
.goods .foods-wrapper .food-list .food-item .food-content .name{
  font-size: 16px;
  margin-bottom: 6px;
  font-weight: bolder;
  display: -webkit-box;
  -webkit-line-clamp:1;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;

}
.goods .foods-wrapper .food-list .food-item .food-content .extra{
  font-size: 14px;
  color:#1d0c0c93;
  margin-bottom: 6px;
}
.goods .foods-wrapper .food-list .food-item .food-content .extra .saled{
  margin-right: 7px;
}
.goods .foods-wrapper .food-list .food-item .food-content .price .text{
  color:red;
  font-size: 15px;
}
.goods .foods-wrapper .food-list .food-item .food-content .price .unit{
    color:#1d0c0c93;
    font-size: 15px;
}
/* 左侧滚动时样式 */
.goods .menu-wrapper .current{
    background-color: #ffffff;
    font-weight: bolder;
   
}
</style>
