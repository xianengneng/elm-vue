<template>
   <div class="ratings-pinjia" ref="ratings">
   	 <div class="pinjia-content">
   	 	<div class="pinjia-overview">
   	 		<div class="overview-left">
   	 			<h1 class="overview-score">{{seller.score}}</h1>
   	 			<div class="overview-title">综合评分</div>
   	 			<div class="overview-rank">高于周边商家{{seller.rankRate}}</div>
   	 		</div>
   	 		<div class="overview-right">
   	 			<div class="score-wrapper">
   	 				<span class="score-title">服务态度</span>
                    <star class="star-ratings" :size="24" :score="seller.serviceScore"></star>
                    <span class="score-score">{{seller.serviceScore}}</span>
   	 			</div>
   	 			<div class="score-wrapper">
   	 				<span class="score-title">商品评分</span>
                    <star class="star-ratings" :size="24" :score="seller.foodScore"></star>
                    <span class="score-score">{{seller.foodScore}}</span>
   	 			</div>
   	 			<div clas="overview-delivery">
   	 				<span class="delivery-title">送达时间</span>
   	 				<span class="delivery-time">{{seller.deliveryTime}}分钟</span>
   	 			</div>
   	 		</div>
   	 	</div>
   	 	<split></split>
   	 	 <ratingselect :ratings='ratings'></ratingselect>
   	 	 <div class="ratings-waichen">
   	 	 	<ul>
   	 	 		<li class="rating-iten" v-for="rating in ratings">
   	 	 			<div class="rating-iten-avater">
   	 	 				<img :src="rating.avatar" width="28" height="28">
   	 	 			</div>
   	 	 			<div class="rating-iten-content">
   	 	 				<h1 class="rating-iten-name">{{rating.username}}</h1>
   	 	 				 <span class="rateTime">{{rating.rateTime}}</span>
   	 	 				<div class="rating-iten-star">
   	 	 					<star class="rating-iten-starstar" :size="24" :score="rating.score"></star>
   	 	 					<span class="rating-iten-deliveryTime">{{rating.deliveryTime}}分钟送达</span>
   	 	 				</div>
   	 	 				 <div class="rating-iten-text">
                  {{rating.text}}
                </div>
                <div class="recommend">
                  <span class="icon-like" v-show="rating.recommend.length"></span>
                  <span class="dish" v-for="dish in rating.recommend">{{dish}}</span>
                </div>
   	 	 			</div>
   	 	 		</li>
   	 	 	</ul>
   	 	 </div>
   	 </div>

   </div>
</template>

<script>
import BScroll from 'better-scroll'
import star from 'components/star/star.vue'
import split from 'components/split/split.vue'
import ratingselect from 'components/ratingselect/ratingselect.vue'
import axios from 'axios'


export default {
  props:{
  	seller:{
  		type:Object
  	}
  },
  data(){
    return{
      ratings:[]
    }
  },
  created(){
    axios.get('static/data.json').then((res) => {
    	this.ratings=res.data.ratings;
    	this.$nextTick(() => {
    		this.scroll = new BScroll(this.$refs.ratings,{
    		click:true
    	})
    	})
    	
    })
  },
  components:{
  	star,
  	split,
  	ratingselect,
  	
  }
}
</script>

<style>
.ratings-pinjia{
	position:absolute;
	top:174px;
	bottom:0;
	left:0;
	width:100%;
	overflow:hidden;

}
.pinjia-overview{
	display:flex;
	padding:18px 0;

}
.overview-left{
   flex:0 0 137px;
   width:137px;
   border-right:1px solid rgba(7,17,27,0.4);
   text-align:center;
   padding:6px 0;
}
.overview-right{
	flex:1;
	padding: 6px 0 6px 24px;
}
.overview-score{
	line-height:28px;
	font-size:24px;
	color:rgb(255,153,0);
	margin-bottom:6px;
}
.overview-title{
	font-size:12px;
	line-height:12px;
	color:rgb(7,17,27);
	margin-bottom:8px;
}
.overview-rank{
	font-size:10px;
	line-height:10px;
	color:rgb(147,153,159);
}
.score-wrapper{
  
   margin-bottom:8px;
   font-size:0;
}
.score-title{
   line-height:18px;
   display:inline-block;
   vertical-align:top;
   font-size:12px;
   color:rgb(7,17,27);
}
.score-score{
	 line-height:18px;
   display:inline-block;
   vertical-align:top;
   font-size:12px;
   color:rgb(255,153,0);
}
.star-ratings{
	vertical-align:top;
	display:inline-block;
	margin:0 12px;
}
.overview-delivery{
	font-size:0;
	
}
.delivery-title{
	line-height:18px;
   font-size:12px;
   color:rgb(7,17,27);
}
.delivery-time{
	font-size:12px;
	line-height:18px;
	color:rgb(147,153,159);
	margin:0 12px;
}
 .rating-iten{
      display: flex;
      padding: 18px 0;
      margin: 0 18px;
      border-bottom: 1px solid rgba(7,17,27,0.1);
  }
.rating-iten-avater{
	  flex: 0 0 28px;
	  margin-right: 12px;
}
.rating-iten-avater img{
      border-radius:50%;
  }
.rating-iten-content{ 
      flex: 1;
}
       
.rating-iten-name{
	 font-size: 10px;
     color: rgb(7,17,27);
     line-height: 12px;
}
          
.rateTime{

	font-size:10px;
	 position: absolute;
     font-weight: 200;
     right: 18px;
     color: rgb(147,153,159);
}
            
.rating-iten-star{
	font-size: 0;
    padding-top: 4px;
    margin-bottom: 6px;
}
          
.rating-iten-starstar{
	vertical-align:top;
	display: inline-block;
}
            
.rating-iten-deliveryTime{
	vertical-align:top;
	display:inline-block;
    font-size: 10px;
    padding-left: 6px;
    font-weight: 200;
    color: rgb(147,153,159);
}
.rating-iten-text{
	font-size: 12px;
    color: rgb(7,17,27);
    line-height: 18px;
}
          
.recommend{
   padding-top: 4px;
}
.icon-like{
	font-size: 12px;
    color: rgb(0,160,220);
    line-height: 16px;
}
            
.dish{
	display: inline-block;
	font-size: 9px;
	color: rgb(147,153,159);
	line-height: 16px;
	border: 1px solid rgba(7,17,27,0.1);
	padding: 2px 6px;
	margin-right: 8px;
	white-space: normal;
	margin-top: 4px;
}


</style>
