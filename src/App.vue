<template>
  <div>
    <v-header :seller="seller"></v-header> <!--:seller 是v-bind: seller...的缩写-->
    <div class="tab border-1px">
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
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script type="text/ecmascript-6">
  import header from './components/header/header.vue';
  import {urlParse} from './common/js/util';

  const ERR_OK = 0;

  export default {
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
    created() { // 生命周期钩子，created钩子
      const url = 'api/seller';
      this.$http.get(url + '?id=' + this.seller.id).then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.seller = Object.assign({}, this.seller, response.data); /// 通过这种方式给object赋值可以直接在原object上新增键值对
        }
      });
    },
    components: {
      'v-header': header
    }
  };
</script>

<style lang="stylus"
       rel="stylesheet/stylus">
  @import "./common/stylus/mixin.styl" // stylus 特殊语法
  .tab
    display: flex
    width: 100%
    height: 40px
    line-height: 40px
    border-1px(rgba(7, 17, 27, 0.1))
    .tab-item
      flex: 1
      text-align: center
      & > a
        display: block
        font-size: 14px
        color: rgb(77, 85, 93)
        &.router-link-active
          color: rgb(240, 20, 20)
</style>
