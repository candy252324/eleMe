<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="supports-count">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <!--<div class="bulletin-wrapper" @click="showDetail">-->
      <!--<span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>-->
      <!--<i class="icon iconfont icon-zuoyoujiantou"></i>-->
    <!--</div>-->
    <!--<div class="background">-->
      <!--<img :src="seller.avatar" alt="" class="" width="100%" height="100%">-->
    <!--</div>-->
    <!--<transition name="fade">-->
      <!--<div v-show="detailShow" class="detail" @click="hideDetail" transition="fade">-->
        <!--<div class="detail-wrapper clearFix">-->
          <!--<div class="detail-main">-->
            <!--<h1 class="name">{{seller.name}}</h1>-->
            <!--<div class="star-wrapper">-->
              <!--<star :size="48" :score="seller.score"></star>-->
            <!--</div>-->
            <!--<div class="title">-->
              <!--<div class="line"></div>-->
              <!--<div class="text">优惠信息</div>-->
              <!--<div class="line"></div>-->
            <!--</div>-->
            <!--<ul v-if="seller.supports" class="supports">-->
              <!--<li class="support-item" v-for="(item, index) in seller.supports">-->
                <!--<span class="icon" :class="classMap[seller.supports[index].type]"></span>-->
                <!--<span class="text">{{seller.supports[index].description}}</span>-->
              <!--</li>-->
            <!--</ul>-->
            <!--<div class="title">-->
              <!--<div class="line"></div>-->
              <!--<div class="text">商家公告</div>-->
              <!--<div class="line"></div>-->
            <!--</div>-->
            <!--<div class="bulletin">-->
              <!--<p class="content">{{seller.bulletin}}</p>-->
            <!--</div>-->
          <!--</div>-->
        <!--</div>-->
        <!--<div class="detail-close" @click="hideDetail">-->
          <!--<i class="iconfont icon-cha"></i>-->
        <!--</div>-->
      <!--</div>-->
    <!--</transition>-->
  </div>
</template>

<script>
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {}
    },
    created(){
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    methods:{
      showDetail(){}
    }
  }
</script>

<style scoped lang="stylus">
  @import '../../common/stylus/mixin.styl'
  .header
    position relative
    color: #fff
    overflow hidden
    background: rgba(7, 17, 27, 0.5)
    .content-wrapper
      position relative
      padding: 24px 12px 18px 24px
      font-size: 0
      .avatar
        display: inline-block
        vertical-align top
        img
          border-radius 2px
      .content
        display: inline-block
        margin-left 16px
        .title
          margin 2px 0 8px 0
          .brand
            display inline-block
            vertical-align top
            width 30px
            height 18px
            bg-image('brand')
            background-size 30px 18px
            background-repeat no-repeat
          .name
            margin-left 6px
            font-size 16px
            line-height 18px
            font-weight bold
        .description
          margin-bottom 10px
          line-height 12px
          font-size 12px
        .support
          .icon
            display inline-block
            width 12px
            height 12px
            vertical-align top
            margin-right 4px
            background-size 12px 12px
            background-repeat no-repeat
            &.decrease
              bg-image('decrease_1')
            &.discount
              bg-image('discount_1')
            &.guarantee
              bg-image('guarantee_1')
            &.invoice
              bg-image('invoice_1')
            &.special
              bg-image('special_1')
          .text
            line-height 12px
            font-size 10px
      .supports-count
        position absolute
        right 12px
        bottom 14px
        padding 0 8px
        height 24px
        line-height 24px
        border-radius 12px
        background rgba(0, 0, 0, 0.2)
        text-align content
        .count
          font-size 10px
          vertical-align top
        .icon.icon-keyboard_arrow_right
          margin-left 2px
          line-height 24px
          font-size 10px
</style>
