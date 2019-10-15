<template>
  <div class="cartControl">
    <!--加减号  -->
      <div class="cart-dele">
          <transition name="move">
        <i class="iconfont" @click.stop.prevent="decreaseCart" v-show="food.count">&#xe600;</i>
            </transition>
      </div>
   
      <div class="cart-number" v-show="food.count">{{food.count}}</div>
      <div class="cart-add">
        <div class="card-bg">
          <i class="iconfont" @click.stop.prevent="increaseCart">&#xe604;</i>
        </div>
      </div>
   
  </div>
</template>

<script>
import Vue from "vue";
// 使用vue的set方法
export default {
  props: {
    food: {
      type: Object
    },
    
  },
  methods: {
    decreaseCart() {
      if (this.food.count > 0) {
        this.food.count--;
      }
    },
    increaseCart() {
      if (!this.food.count) {
        Vue.set(this.food, "count", 1); //往数据(this.food对象）里添加count:1
      } else {
        this.food.count++;
      }
    }
  }
};
</script>


<style scoped>
.cartControl {
  width: 78px;
  height: 30px;
  line-height: 30px;
  text-align: center;
}
.cartControl .cart-dele i {
  font-size: 18px;
  color: #b4b4bb;
  float: left;
}
.cartControl .cart-add {
  float: right;
  margin-right: 20px;
  color: #ffd161;
}
.cartControl .cart-add i {
  font-size: 18px;
  margin-left: -2px;
}
.cartControl .cart-number {
  float: left;
  margin-left: 5px;
}
.cartControl .cart-add .card-bg {
  width: 13px;
  height: 13px;
  line-height: 13px;
  border-radius: 50%;
  background: black;
  display: inline-block;
}
.move-enter-active,.move-leave-active{
    transition: all 0.3s linear;
}
.move-enter,.move-leave-to{
    transform: translateX(20px) rotate(180deg);
}
</style>
 