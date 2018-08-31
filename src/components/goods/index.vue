<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(item, index) in goods" class="menu-item border-1px" :class="{'current':currentIndex === index}"
            @click="selectMenu(index, $event)">
          <span class="text">
            <span v-show="item.type>0" class=" icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodWrapper">
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item" @click="selectFood(food, $event)">
              <div class="icon">
                <img :src="food.icon" alt="" width="57">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}</span><span class="count">好评{{food.rating}}</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span><span class="old"
                                                                v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
                <div class="cartControl-wrapper">
                  111111
                  <!--<cartControl :food="food" @increment="incrementTotal"></cartControl>-->
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  const ERR_OK=0;

  export default {
    name:"goods",
    data() {
      return {
        goods:[],
        currentIndex:undefined,
      }
    },
    created(){
      this.axios.get("/api/goods").then(res=>{
        let d=res.data
        if(d.errno===ERR_OK){
          this.goods=d.data;
        }
      })
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    methods:{
      selectMenu(){},
      selectFood(){},
    }
  }
</script>

<style scoped lang="stylus">
  @import "goods.styl";
</style>
