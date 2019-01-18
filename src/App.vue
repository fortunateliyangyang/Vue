<template>
  <div class="App">
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item ">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
   
    </div>
    <!-- 保存组件状态 -->
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
    <!-- 路由出口 -->
    <!-- 路由匹配到的组件将渲染在这里 -->
</div>
</template>

<script type="text/javascript">
    import header from './components/header/header.vue';
    import {urlParse} from './common/js/util';

    const ERR_OK = 0;
    export default{
      data(){
        return {
           seller:{
              id:(()=>{
                 let queryParam = urlParse();
                 console.log(queryParam);
                 return queryParam.id;
              })()
           },

        }
      },
      components:{
        'v-header':header
      },
      created(){
       this.$http.get('./api/seller?id='+this.seller.id).then((res)=>{
         var resData  = res.data;
         if(resData.errno  === ERR_OK){
           /* this.seller =  resData.data; 会覆盖掉id */
           /* 防止把id覆盖掉，使用es6的一个语法:扩展了对象的属性，在原来的基础上添加response.data的值，不会覆盖掉原来的id属性 */
           this.seller = Object.assign({},this.seller,resData.data);
           console.log(this.seller.id);
         }
      },(res)=>{
         alert(res.status);
       });
    },
    }
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "./common/stylus/mixin.styl"
.App
  z-index:999 
  .tab
     display:flex
     width:100%
     height:30px
     line-height:30px
     border-1px(rgb(7,17,27,0.1)) 
  .tab-item
         flex:1
         text-align:center
         &>a 
          display:block
          font-size:14px
          color:rgb(77,85,93)
          text-decoration:none
          &.active
           color:rgb(240, 20, 20)
</style>
