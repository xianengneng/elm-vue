<template>
  <transition name="move">
   <div v-show="showFlag" class="detail-wrapper" ref="detailWrapper">
   	<div class="detail-content">
      <div class="image-header">
        <img :src="food.image">
        <div class="iconundo" @click="hide"><i class="icon-undo"></i></div>
      </div>
      <div class="deta-content">
        <h1 class="deta-titel">{{food.name}}</h1>
        <div class="deta-detail">
          <span class="deta-sell">月售{{food.sellCount}}份</span>
          <span class="deta-ratings">好评率{{food.rating}}%</span>
        </div>
        <div class="deta-price">
          <span class="deta-newPrice"><span class="unit">￥</span>{{food.price}}</span><span v-show="food.oldPrice" class="deta-oldPrice">￥{{food.oldPrice}}</span>
        </div>
        <div class="cartcontrol-wrapperer">
        <cartcontrol :food="food"></cartcontrol>
      </div>
      <div class="buy" v-show="!food.count || food.count===0" @click.stop.prevent="addFirst">加入购物车
      </div>
      </div>
   <split v-show="food.info"></split>
   <div class="info" v-show="food.info">
     <h1 class="info-title">商品信息</h1>
     <p class="info-text">{{food.info}}</p>
   </div> 
   <split></split>
   <div class="ratinginfo">
     <h1 class="ratinginfo-title">商品评价</h1>
     <ratingselect :selectType='selectType' :onlyContent='onlyContent' :desc='desc' :ratings='food.ratings'></ratingselect>
     <div class="rating-wai">
       <ul v-show="food.ratings && food.ratings.length">
         <li v-show="needShow(rating.rateType,rating.text)" v-for="rating in food.ratings" class="rating-item">
           <div class="rating-user">
             <span class="user-name">{{rating.username}}</span>
             <img class="user-avatar" width="12" height="12" :src="rating.avatar">
           </div>
           <div class="time">{{rating.rateTime}}</div>
           <p class="user-text">{{rating.text}}</p>
         </li>
       </ul>
       <div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
     </div>
   </div>  
    </div>
   </div>
   </transition>
</template>

<script>
import BScroll from 'better-scroll'
import cartcontrol from 'components/cartcontrol/cartcontrol.vue'
import Vue from 'vue'
import split from 'components/split/split.vue'
import ratingselect from 'components/ratingselect/ratingselect.vue'



const POSITIVE = 0;
const NEGATIVE = 1;
const ALL = 2;
export default {
  props:{
  	food:{
  		type:Object
  	}
  },
  components:{
     cartcontrol,
     split,
     ratingselect
  },
  data(){
    return{
      showFlag:false,
      selectType:ALL,
      onlyContent:true,
      desc:{
        all:'全部',
        positive:'推荐',
        negative:'吐槽'
      }
    }
  },
  methods: {
    showToggle(){
      this.showFlag=!this.showFlag;
      this.selectType = ALL;
      this.onlyContent = true;
      this.$nextTick(() => {
        if(!this.scroll){
          this.scroll=new BScroll(this.$refs.detailWrapper,{
            click:true
          })
        }else{
          this.scroll.refresh();
        }
      })
    },
    hide(){
      this.showFlag=false;
    },
    addFirst(event){
     if(!event._constructed){
      return
     }else{
      Vue.set(this.food,'count',1);
     }
    },
    needShow(type,text){
      if(this.onlyContent && !text){
        return false;
      }
      if(this.selectType === ALL){
        return true
      }else {
        return type===this.selectType;
      }
    }
   

  }
}
</script>

<style>
.detail-wrapper{
  transition:all 0.4s linear;
	position:fixed;
	left:0px;
	top:0px;
	bottom:48px;
	z-index:30px;
	width:100%;
	background:#fff;
 
}
.detail-wrapper.move-transition{
  transform:translate3d(0,0,0);
  
}
.detail-wrapper.move-enter, .detail-wrapper.move-leave{
    transform:translate3d(100%,0,0);
}
.image-header{
   position:relative;
   width:100%;
   height:0;
   padding-top:100%;

}
.image-header img{
  position:absolute;
  top:0;
  left:0;
  width:100%;
  height:100%;
}

.detail-content{

}
.iconundo{
  position:absolute;
  top:10px;
  left:0;
}
.icon-undo{
  display:block;
  padding:10px;
  font-size:20px;
  color:#fff;

}
.deta-content{
   padding:18px;
   position:relative;
}
.deta-titel{
   line-hight:14px;
   margin-bottom:8px;
   font-size:14px;
   font-weight:700;
   color:rgb(7,17,27);
}
.deta-detail{
   margin-bottom:18px;
   line-hight:10px;
   font-size:0px;
   height:10px;
}
.deta-sell{
   font-size:10px;
   color:rgb(147,153,159);
   margin-right:12px;
}
.deta-ratings{
font-size:10px;
   color:rgb(147,153,159);
}
.deta-price{
  font-weight:700;
  line-hight:24px;

}
.deta-newPrice{
  margin-right:18px;
  font-size:14px;
  color:rgb(240,10,10);

}
.deta-oldPrice{
  text-decoration:line-through;
  font-size:10px;
  color:rgb(147,153,159);

}
.cartcontrol-wrapperer{
    position:absolute;
    right:12px;
    bottom:12px;
}
.buy{
position:absolute;
bottom:18px;
right:18px;
z-index:10;
height:24px;
background:rgb(0,160,220);
padding:0 12px;
box-sizing:border-box;
font-size:10px;
color:#fff;
border-radius:12px;
text-align:center;
line-height:24px;

}
.info{
  padding:18px;
}
.info-title{
  line-hight:14px;
  margin-bottom:16px;
  font-size:14px;
  color:rgb(7,17,27);
}
.info-text{
  color:rgb(77,83,93);
  line-height:24px;
  padding:0 8px;
  font-size:12px;

}
.ratinginfo{
padding-top:18px;
}
.ratinginfo-title{
 line-hight:14px;
  margin-left:18px;
  font-size:14px;
  color:rgb(7,17,27);

}
.rating-wai{
  padding:0 18px;

}
.rating-item{
  position:relative;
  padding:16px 0;
}
.rating-user{
  position:absolute;
  right:0;
  top:16px;
  font-size:0;
  line-height:12px;
}
.user-name{
  display:inline-block;
  vertical-align:top;
  font-size:10px;
  color:rgb(147,153,159);
  margin-right:6px;
}
.user-avatar{
  display:inline-block;
}
.time{
  margin-bottom:6px;
  line-height:12px;
  font-size:10px;
  color:rgb(147,153,159);
}
.user-text{
  line-height:16px;
  font-size:12px;
  color:rgb(7,17,27);
}
.no-rating{
  padding:16px 0;
  font-size:12px;
  color:rgb(147,153,159);
}
</style>
