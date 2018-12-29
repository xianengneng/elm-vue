<template>
 <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
      	<li v-for="(item,index) in goods" @click="seletMenu(index,$event)"  :class="index==CurrentIndex?'menu-item-selected':'menu-item'">
      		<span class="menu-text">
      		  <span v-show="item.type>0" class="menu-icon":class="ClassMap[item.type]"></span>{{item.name}} 
            </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" id="wrapper" ref="foodsWrapper">
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook">
          <h1 class="foods-title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item" @click="goDetail(food)">
              <div class="food-icon">
                <img width="57" height="57" :src="food.icon"/>
              </div>
              <div class="food-content">
	                <h2 class="foods-name">{{food.name}}</h2>
	                <p class="food-description" v-show="food.description">{{food.description}}</p>
	                <div class="sell-info">
	                  <span class="sellCount">月售{{food.sellCount}}份</span>
	                  <span class="food-rating">好评率{{food.rating}}%</span>
	                </div>
	                <div class="food-price">
	                  <span class="newPrice"><span class="unit">￥</span>{{food.price}}</span>
	                  <span v-show="food.oldPrice" class="oldPrice">￥{{food.oldPrice}}</span>
	                </div>
	                 <div class="cartcontrol-wrapper">
                     <cartcontrol :food="food"></cartcontrol>
                    </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <v-shopcart :selectFoods="selectFoods" :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"></v-shopcart>
    <foodDetail :food="selectedFood" v-if="selectedFood" ref="myFood"></foodDetail>
 </div>
</template>

<script>

import BScroll from 'better-scroll'
import axios from 'axios'
import Vue from 'vue'
import shopcart from 'components/shopcart/shopcart.vue'
import cartcontrol from 'components/cartcontrol/cartcontrol.vue'
import foodDetail from 'components/foodDetail/foodDetail.vue'
export default {
  props:{
  	seller:{
  		type:Object
  	} 	
  },
  data(){
     return{
     	goods:[],
     	listHeight:[],
     	scrollY: 0,
     	selectedFood:{}
     }
  },
  components:{
  	"v-shopcart":shopcart,
  	cartcontrol,
  	foodDetail
  },
  computed:{
     CurrentIndex(){
     	for(var i=0;i<this.listHeight.length;i++){
     		let height1=this.listHeight[i];
     		let height2=this.listHeight[i +1];
     		if(!height2 || (this.scrollY >=height1 && this.scrollY <height2)){
                return i;
     		}
     	}
     	return 0;
     },
    selectFoods(){
    	let foods=[];
    	this.goods.forEach((good) => {
    		good.foods.forEach((food) =>{
    			if(food.count){
    				foods.push(food);
    			}
    		})
    	})
    	return foods;
    }
  },
  
  created() {
    axios.get('static/data.json').then((res) => {
      this.goods = res.data.goods;
      this.$nextTick(() => {
      this._initScroll(); // 初始化scroll
      this._calculateHeight();
      })
    }),
    this.ClassMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
 },
 methods:{
 	_initScroll(){
 		this.menuScroll = new BScroll(this.$refs.menuWrapper,{ click:true
 		})
 		this.foodsScroll = new BScroll(this.$refs.foodsWrapper,{
 			click:true,
 			probeType: 3
 		})
 		this.foodsScroll.on('scroll',(pos) => {
 			this.scrollY= Math.abs(Math.round(pos.y))
 		})
 	},
 	_calculateHeight(){
        let foodList=this.$refs.foodsWrapper.querySelectorAll('.food-list-hook');
        let height=0;
        this.listHeight.push(height);
        for(let i=0;i<foodList.length;i++){
        	let item=foodList[i];
        	height += item.clientHeight;
        	this.listHeight.push(height);
        }

 	},
 	seletMenu(index,event){
 		if(!event._constructed){
 			return
         }
         this.foodsScroll.scrollTo(0, -this.listHeight[index], 300)
 		

 	},
 	goDetail(food) {
      this.selectedFood = food
      this.$nextTick(() => {
        this.$refs.myFood.showToggle()
      })
    }

  }

}
</script>

<style>
.decrease{
	background-image:url('decrease_3@3x.png');
	display:inline-block;
}
.guarantee{
	background-image:url('guarantee_3@3x.png');
	display:inline-block;
}
.invoice{
	background-image:url('invoice_3@2x.png');
	display:inline-block;
}
.special{
	background-image:url('special_3@2x.png');
	display:inline-block;
}

.goods{
	display:flex;
	position:absolute;
	width:100%;
	top:175px;
	bottom:46px;
	overflow:hidden;
}
.menu-wrapper{
	flex:0 0 80px;
	width:80px;
	background:#f3f5f7;
	margin-top:2px;

}
.menu-item,.menu-item-selected{
	position:relative;
	display:table;
	line-hight:14px;
	width:56px;
	height:54px;
	padding:0 12px;
}
.menu-item,.menu-item-selected:last-child{
	content:none;
}
.menu-item-selected{
        background: white;
        font-weight: 700;
        margin-top: -1px;
 }
.menu-item:after{
      position: absolute;
      content: '';
      left: 12px;
      width: 56px;
      bottom: 0;
      border-bottom: 1px solid rgba(7,17,27,0.1);
 }
.menu-icon{
	display:inline-block;
	vertical-align:top;
	width:12px;
	height:12px;
	margin-right:2px;
	background-size:12px 12px;
	background-repeact:no-repeact;
}
.menu-text{       
  display: table-cell;
  vertical-align: middle;
  font-size: 12px;
  font-weight: 700;
  width:56px;
  white-space: normal;
  line-height: 14px;

} 
.foods-wrapper{
 flex:1;

}
.foods-title{
	padding-left:14px;
	height:26px;
	line-hight:26px;
	border-left:2px  solid #d9dde1;
	font-size:12px;
	color:rgb(147,153,159);
	background:#f3f5f7;

}
.food-item{
	display:flex;
	margin:18px;
	padding:18px 0;
	
}
.food-item:last-child{
	margin-bottom:0px;
}
.food-icon{
	flex:0 0 57px;
	margin-right:10px;

}
.food-content{
	flex:1;
	position:relative;
	padding-left: 10px;

}
.foods-name{
	font-size:14px;
	margin:2px 0 8px 0;
	height:14px;
	line-hight:14px;
	color:rgb(7,17,27);
}
.food-description{
	line-hight:10px;
	font-size:10px;
	color:rgb(147,153,159);
	margin-bottom:8px;

}
.sell-info{
	line-hight:10px;
	font-size:10px;
	color:rgb(147,153,159);
}
.sellCount{
	margin-right:12px;
}
.food-price{
	font-weight:700;
	line-hight:24px;

}
.newPrice{
	margin-right:18px;
	font-size:14px;
	color:rgb(240,10,10);

}
.oldPrice{
	text-decoration:line-through;
	font-size:10px;
	color:rgb(147,153,159);

}
.cartcontrol-wrapper{
	position:absolute;
	bottom:0px;
	right:0;
	z-index: 20;

}
 

</style>
