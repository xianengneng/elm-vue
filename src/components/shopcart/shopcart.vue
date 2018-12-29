<template>
   <div class="shopcart">
   	 <div class="cart-content" @click="toggleList">
   	 	<div class="content-left">
   	 		<div class="logo-wrapper">
   	 			<div class="logo" :class="{'hightlight':totalCount>0 }">
   	 			  <i class="icon-gouwuche" :class="{'hightlight':totalCount>0 }"></i>
   	 			</div>
   	 			<div class="number" v-show="totalCount>0">{{totalCount}}</div>
   	 		</div>
   	 		<div class="cart-price" :class="{'hight':totalPrice>0}">￥{{totalPrice}}元</div>
   	 		<div class="cart-desc">外需配送费￥{{deliveryPrice}}元</div>
   	 	</div>
   	 	<div class="content-right" @click.stop.prevent="pay">
   	 		<div class="pay" :class="payClass">{{payDesc}}</div>
   	 	</div>
   	 </div>
     <div class="shopcart-list" v-show="listShow">
       <div class="list-header">
         <h1 class="list-title">购物车</h1>
         <span class="empty" @click="empty">清空</span>
       </div>
       <div class="list-content" ref="listContent">
         <ul>
           <li class="list-foods" v-for="food in selectFoods">
             <span class="foodname">{{food.name}}</span>
             <div class="foodprice"><span>￥{{food.price*food.count}}</span></div>
             <div class="cartcontrol-foodwrapper">
              <cartcontrol :food="food"></cartcontrol>
             </div>
           </li>
         </ul>
       </div>
     </div>
   </div>
  
</template>

<script>
import cartcontrol from 'components/cartcontrol/cartcontrol.vue'
import BScroll from 'better-scroll'
export default {
	props:{
   deliveryPrice:{
   	type:Number,
   	default:0
   },
  minPrice:{
   	type:Number,
   	default:0
   },
   selectFoods:{
   	type:Array,
   	default(){
   		return [{
        price:10,
   		  count:4
   	}];
   	}
   }
	},
  data(){
    return {
      fold:true
    }
  },
  components:{
    cartcontrol
  },
	computed:{
		totalPrice(){
			let total=0;
			this.selectFoods.forEach((food) => {
				total+=food.price*food.count;
			})
			return total
		},
		totalCount(){
			let count=0;
			this.selectFoods.forEach((food) => {
				count+=food.count;
			})
			return count
		},
		 payDesc() {
      let diff = this.minPrice - this.totalPrice
      if (!this.totalPrice) {
        return `￥${this.totalPrice}起送`
      } else if (diff > 0) {
        return `还差￥${diff}元`
      } else {
        return '去结算'
      }
    },
    payClass(){
    if(this.totalPrice<this.minPrice){
    return 'not-enough';
      }else{
      return 'enough';
    }
  },
   listShow(){
      if(!this.totalCount){
        this.fold=true;
        return false;
      }
      let show=!this.fold
      if(show){
        this.$nextTick(() => {
          if(!this.scroll){
          this.scroll=new BScroll(this.$refs.listContent,{
            click:true
          })
        }else{
          this.scroll.refresh();
        }
        })
      }
      return show;
   }
},
methods:{
  toggleList(){
    if(!this.totalCount){
      return;
    }
    this.fold=!this.fold;
  },
  empty(){
  this.selectFoods.forEach((food) => {
    food.count = 0;
  })
},
  pay(){
    if(this.totalPrice<this.minPrice){
      return 
    }
    window.alert(`你需要支付${this.totalPrice}元`)
  }
}


  
}
</script>

<style>
.shopcart{
	width:100%;
	position:fixed;
	bottom:0px;
	z-index:50;
	height:48px;
	background:#ccc;
}
.cart-content{
	display:flex;
	background:#141d27;
	font-size:0px;
}
.content-left{
	flex:1;
}
.content-right{
    flex:0 0 105px;
    width:105px;
}
.pay, .not-enough, .enough{
	font-size:12px;
	height:48px;
	line-height:48px;
	text-align:center;
	font-weight:700;
	color:rgba(255,255,255,0.4);
	background:#2b333b;
}
.not-enough{
	background:#2b333b;
}
.enough{
	background: #00b43c;
	color: #fff;
}
.logo-wrapper{
   display:inline-block;
   position:relative;
   top:-10px;
   margin:0 12px;
   padding:6px;
   width:56px;
   height:56px;
   box-size:border-box;
   vertical-align:top;
   border-radius:50%;
   background:#141d27;
}
.logo,.hightlight{
   height:100%;
   width:100%;
   border-radius:50%;
   background:#2b343c;
   text-align:center;
}
.hightlight{
	background:rgb(0,160,220);
}
 .icon-gouwuche, .hightlight{
	font-size:24px;
	color:#80858a;
  line-height: 44px;
	
}
.hightlight{
    color:#fff;
}
.cart-price, .light{
    display:inline-block;
    vertical-align:top;
    margin-top:12px;
    line-height:24px;
    padding-right:12px;
    border-right:1px solid rgba(255,255,255,0.1);
    font-size:16px;
    font-weight:700;
    color:rgba(255,255,255,0.4);
    box-sizing:border-box;
    
}
.hight{
    color:#fff;
}


.cart-desc{
    display:inline-block;
    vertical-align:top;  
    margin:12px 0 0 12px;
    line-height:24px;
    font-size:10px;
    font-weight:700;
    color:rgba(255,255,255,0.4);
}
.number{
	position:absolute;
	top:0px;
	right:0px;
	width:24px;
	height:16px;
	line-height:16px;
	text-align:center;
	border-radius:16px;
	font-size:9px;
	font-weight:700;
	color:#fff;
	background:rgb(240,20,20);
	box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4);
}
.shopcart-list{
  position:absolute;
  bottom:0px;
  left:0;
  z-index:-1;
  background:white;
  width:100%;
  margin-bottom:48px;
}
.list-header{
  height:40px;
  line-height:40px;
  background:#f3f5f7;
  border-bottom:1px solid rgba(7,17,27,0.1);
}
.list-title{
  display:inline-block;
  font-size:14px;
  font-size:200;
  color:rgb(7,17,27);
  padding-left:18px;
}
.empty{
  position:absolute;
  padding:0 10px;
  right:8px;
  font-size:12px;
  color:rgb(0,160,220);

}
.list-content{
  max-height:250px;
  overflow:hidden;
  background:#fff;
}
.list-food{
display: flex;
posiyion:relative;
height:48px;
margin:0 18px;
border-bottom:1px solid rgba(7,17,27,0.1);

}
.foodname{
  margin-left:20px;
  display:inline-block;
  flex:1;
  font-weight:700;
  line-height:48px;
  font-size:14px;
  color:rgb(7,17,27);
}
.foodprice{
   display:inline-block;
    position:absolute;
    right:120px;
    line-height:48px;
    font-size:14px;
    font-weight:700;
    color:rgb(140,20,20);
}
.cartcontrol-foodwrapper{
  display:inline-block;
  font-size:14px;
  position:absolute;
  right:20px;  
}


</style>

