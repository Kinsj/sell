<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count>0" @click="decreaseCart">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click="addCart"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue';

  export default {
    props: {
      food: {
        type: Object
      }
    },
    created() {
    },
    methods: {
      addCart(event) {
        if (!event._constructed) { // 防止pc端重复点击goods.vue有详细说明
          return;
        }
        if (!this.food.count) {
          // this.food.count = 1; // vue.js中给原本的Object中不存在的属性赋值时是没有效果的
          // 应该用下面的方式给Object 添加新元素并赋值
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count++;
        }
      },
      decreaseCart(event) {
        if (!event._constructed) { // 防止pc端重复点击goods.vue有详细说明
          return;
        }
        this.food.count--;
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size: 0
    .move-enter-active, .move-leave-active
      opacity: 1
      transform: translate3d(0, 0, 0)
    .move-enter, .move-leave-to
      opacity: 0
      transform: translate3d(24px, 0, 0)
    .cart-decrease
      display: inline-block
      padding: 6px
      transition: all 0.4s linear
      .move-enter-active, .move-leave-active
        transform: rotate(0)
      .move-enter, .move-leave-to
        transform: rotate(180)
      .inner
        display: inline-block
        line-height: 24px
        font-size: 24px
        color: rgb(0, 160, 220)
        transition: all 0.4s linear
    .cart-count
      display: inline-block
      vertical-align: top
      width: 12px
      padding-top: 6px
      line-height: 24px
      text-align: center
      font-size: 10px
      color: rgb(147, 153, 159)
    .cart-add
      display: inline-block
      padding: 6px
      line-height: 24px
      font-size: 24px
      color: rgb(0, 160, 220)
</style>
