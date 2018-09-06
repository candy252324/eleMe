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
    <div class="foods-wrapper" ref="foodsWrapper">
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
                  <span class="now">￥{{food.price}}</span>
                  <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
                <div class="cartControl-wrapper">
                  <cart-control :food="food" @addCart="addCart"></cart-control>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shop-cart
      ref="ShopCart"
      class-="shop-cart"
      :delivery-price="seller.deliveryPrice"
      :min-price="seller.minPrice"
      :selectFoods="selectFoods"
    ></shop-cart>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'
  import ShopCart from '@/components/shopCart'
  import cartControl from '@/components/cartControl'
  const ERR_OK=0;

  export default {
    name:"goods",
    components:{ ShopCart, cartControl },
    data() {
      return {
        goods:[],
        listHeight:[],  // 每个区间的高度
        scrollY: 0,  // 右侧区域滑动的高度
      }
    },
    props: {
      seller: {
        type: Object
      }
    },
    computed: {
      currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height && this.scrollY < height2)) {
            return i;
          }
        }
        return 0;
      },
      selectFoods() {
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            // cartControl组件中通过$set 给food邦定了count观测属性，所以，只要存在count属性，则说明被加入了购物车
            if (food.count) {
              foods.push(food);
            }
          });
        });
        return foods;
      }
    },
    created(){
      this.axios.get("/api/goods").then(res=>{
        let d=res.data
        if(d.errno===ERR_OK){
          this.goods=d.data;
          this.$nextTick(()=>{
            this._initScroll();
            this._calculateHeight();
          })
        }
      })
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    methods:{
      _initScroll(){
        this.menuScroll=new BScroll(this.$refs.menuWrapper,{
          click: true,   // click:true,向外派发点击事件，但是会导致pc端响应两次点击事件
        })
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
          probeType: 3,
          click: true
        });
        this.foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y));
        });
      },
      _calculateHeight(){
        let foodsList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodsList.length; i++) {
          let item = foodsList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      },
      // 小球动画实现
      _drop(target){
        //$nextTick,体验优化，异步执行下落动画
        this.$nextTick(()=>{
          this.$refs.ShopCart.drop(target)
        })
      },
      selectMenu(index,event){
        // betterScroll派发的click事件,event有_constructed属性，而浏览器原生的click事件没有该属性
        // 从而规避了pc端响应两次点击事件的问题(实际上似乎并不存在这个问题？？？)
        if (!event._constructed) {
          return;
        }
        let foodsList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let el = foodsList[index];
        this.foodsScroll.scrollToElement(el, 300);
      },
      selectFood(){},
      addCart(target){
        this._drop(target)
      },
    }
  }
</script>

<style scoped lang="stylus">
  @import "goods.styl";
</style>
