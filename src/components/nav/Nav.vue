<template>
  <div class="nav">
      <router-link class="nav-item" to="/goods">
          商品
      <i class="line"></i>
      </router-link>
    
      <router-link class="nav-item" to="/seller">
          商家
      <i class="line"></i>
      </router-link>
  </div>
</template>

<script>
export default {
  
}
</script>


<style scoped>
.nav{
  display:flex;
  width: 100%;
  height:40px;
  line-height: 40px;
  padding: 5px 0;
  border-bottom: 1px solid #E4E4E4;
  
}
.nav-item{
  flex:1;
  text-align: center;
  font-size:13px;
  text-decoration: none;
  color: #666;
  position: relative;
}

/* 选中的样式 */
.nav .active{
  color:#ffbb22;
}
.nav .active .line{
  width: 20px;
  height: 2px;
  display: inline-block;
  background:#ffbb22;
  position: absolute;
  left:50%;
  bottom:0;
  margin-left:-10px;
}
</style>
