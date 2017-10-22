<template>
  <div id="app">
    <div class="header">
      <v-header :seller="seller"></v-header>
    </div>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">卖家</router-link>
      </div>
    </div>
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
    <div class="content"></div>
  </div>
</template>

<script>
  import header from './components/header/header.vue';
  import axios from 'axios';
  import {urlParse} from './common/js/util.js';
  const ERR_OK = 0;
  export default {
    data(){
      return {
        seller:{
          id:(()=>{
            let queryParam =urlParse();
            return queryParam.id
          })()
        }
      } 
    },
    created(){
      axios.get('/api/seller').then((response) => {
        response = response.data
        if(response.errno === ERR_OK){
          this.seller = Object.assign({},this.seller,response.data);
        } 

      })
    },
    components:{
      'v-header':header 
    }
  }
</script>
<style lang="scss" type="text/css">
  @import './common/scss/mixin.scss';
  .tab{
    display:flex;
    width:100%;
    height:40px;
    line-height:40px;
    .tab-item{
      flex:1;
      text-align:center;
      // border-bottom: solid 1px rgba(7,17,27,0.1);
      @include border-1px(rgba(7,17,27,0.1));
      & > a{
        display: block;
        font-size:14px;
        color:rgb(77,85,20);
        &.active{
          color: rgb(240,20,20);
        }
      }
    } 
  }
</style>
