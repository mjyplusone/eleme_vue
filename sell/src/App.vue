<template>
  <div>
    <v-header v-bind:seller="seller"></v-header>
    <div class="tab border-1px">
      <!-- 使用v-link进行导航 -->
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <!-- 路由外链 -->
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script type="text/ecmascript-6">
  import header from 'components/header/header.vue';
  import {urlParse} from './common/js/util.js';

  const ERROK = 0;
  
  export default {
    // data必须是函数
    data() {
      return {
        seller: {
          id: (() => {
            let queryParm = urlParse();
            return queryParm.id;
          })()
        }
      };
    },
    created() {
      this.$http.get('/api/seller?id=' + this.seller.id).then((response) => {
        // response = response.json();
        response = response.body;
        // 服务端返回errno为0和data
        if (response.errno === ERROK) {
          // this.seller = response.data;   // 注意不能加这句，加了这句id值会不存在
          this.seller = Object.assign({}, this.seller, response.data);
          console.log(this.seller.id);
        }
      });
    },
    components: {
      // header: header
      // ES6支持简写
      'v-header': header
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixin.styl"

  .tab
    display: flex
    width: 100%
    height: 40px
    line-height: 40px
    // border-bottom: 1px solid rgba(7, 17, 27, 0.1)
    border-1px(rgba(7, 17, 27, 0.1))
    .tab-item
      flex: 1
      text-align: center
      & > a
        display: block
        font-size: 14px
        color: rgb(77, 85 ,93)
        &.active
          color: rgb(240, 20, 20)

</style>
