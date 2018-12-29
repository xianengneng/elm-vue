<template>
	 <div class="shajia-wrapper" ref="sellerWrapperr">
    <div class="sha-content">
      <div class="shajia-info">
        <div class="shajia-title">
          <div class="shajia-text">{{seller.name}}</div>
          <div class="shajia-star-wrapper">
            <star class="shajia-star" :size="36" :score="seller.score"></star>
            <span class="shajia-rate-count">({{seller.ratingCount}})</span>
            <span class="shajia-sell-count">月售{{seller.sellCount}}单</span>
          </div>
          <div class="shajia-collect" @click="collectflag=!collectflag">
            <span class="shajia-icon-favorite"></span>
            <span class="text"></span>
          </div>
        </div>
        <div class="shajia-remark">
          <div class="shajia-block">
            <h2>起送价</h2>
            <div class="shajia-content">
              <span class="shajia-num">{{seller.minPrice}}</span>元
            </div>
          </div>
          <div class="shajia-block">
            <h2>商家配送</h2>
            <div class="shajia-content">
              <span class="shajia-num">{{seller.deliveryPrice}}</span>元
            </div>
          </div>
          <div class="shajia-block">
            <h2>平均配送时间</h2>
            <div class="shajia-content">
              <span class="shajia-num">{{seller.deliveryTime}}</span>分钟
            </div>
          </div>
        </div>
      </div>
      <split></split>
      <div class="jia-activities">
        <div class="jia-bulletin">
          <h1>公告与活动</h1>
          <div class="jia-content">
            {{seller.bulletin}}
          </div>
        </div>
      </div>
      <div class="jia-supports">
        <ul>
          <li class="jia-it" v-for="item in seller.supports">
           
            <span class="jia-item">{{item.description}}</span>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="jia-seller-imgs">
        <h1>商家实景</h1>
        <div class="jia-img-wrapper" ref="picsWrapper">
          <div ref="picList">
            <img v-for="pic in seller.pics" :src="pic" width="120" height="90">
          </div>
        </div>
      </div>
      <split></split>
      <div class="jia-seller-info">
        <h1>商家信息</h1>
        <ul class="jia-info-list">
          <li class="jia-info" v-for="info in seller.infos">{{info}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import BScroll from 'better-scroll'
import star from 'components/star/star'
import split from 'components/split/split.vue'
export default {
 data(){
 	return{
     seller:{}
 	}
 },
components:{
	star,
	split
},
created(){
	axios.get('static/data.json').then((res) => {
    	this.seller=res.data.seller;
    	this.$nextTick(() => {
    		this.scroll = new BScroll(this.$refs.sellerWrapperr,{
    		click:true
    	})
    		this.initpicScroll()
    	})
    	
    })
},
methods:{
	initpicScroll(){
		if(this.picsScroll){
			return 
		}
		const PIC_WIDTH=120
		const MARGIN=6
		let picLen=this.seller.pics.length;
		this.$refs.picList.style.width=PIC_WIDTH*picLen+MARGIN*(picLen-1)+'px';
		this.picsScroll = new BScroll(this.$refs.picsWrapper,{
			scrollX:true
		})
	}
}

}
</script>

<style>
.shajia-wrapper{
  position: absolute;
  top: 174px;
  bottom: 0;
  left: 0;
  width: 100%;
  overflow: hidden;
}
.shajia-info{
	padding:18px 0;
	margin: 0 18px;
}
.shajia-title{
	padding-bottom:18px;
	border-bottom:1px solid rgba(7,17,27,0.1);
}
.shajia-text{
	font-size:14px;
	line-height:14px;
	color:rgb(7,17,27);
	margin-bottom:8px;
}
.shajia-star-wrapper{
	font-size:0;
}
.shajia-rate-count{
	display:inline-block;
	font-size:10px;
	line-height:18px;
	padding:0 12px 0 6px;
	color:rgb(77,85,93);

}
.shajia-sell-count{
    color:rgb(77,85,93);
	display:inline-block;
	font-size:10px;
	line-height:18px;
}
.shajia-star{
	display:inline-block;
	vertical-align:top;
}
.shajia-remark{
	display:flex;
}
.shajia-block{
	text-align:center;
	flex:1;
	padding:18px 0;
    border-right: 1px solid rgba(7,17,27,0.1);
}
.shajia-block:last-child{
	border:none;
}
.shajia-block h2{
	font-size:10px;
	line-height:10px;
	color:rgb(147,153,159);
	padding-bottom:4px;
}
.shajia-content{
	font-size: 10px;
    color: rgb(7,17,27);
    line-height: 24px;
    font-weight: 200;
}
.shajia-num{
	font-size:24px;
}
.jia-activities{
	padding-top:18px;
}
.jia-bulletin{
    margin: 0 18px;
    border-bottom: 1px solid rgba(7,17,27,0.1);
}
.jia-bulletin h1{
    font-size:14px;
	line-height:14px;
	color:rgb(7,17,27);
}
.jia-content{
	font-size:12px;
	line-height:24px;
	color:rgb(240,20,20);
	font-weight:200;
	padding:8px 12px 16px 12px;
}
.jia-supports{
	margin:0 18px;
}
.jia-it{
	padding:16px 12px;
	border-bottom:1px solid rgba(7,17,27,0.1);
}
.jia-it:last-child{
border:none;
}
.jia-item{
	font-size:12px;
	line-height:16px;
	font-weight:200;
	color:rgb(7,17,27);
}
.jia-seller-imgs{
	margin:18px;
}
.jia-seller-imgs h1{
	font-size:14px;
	line-height:14px;
	color:rgb(7,17,27);
	margin-bottom:12px;
}
.jia-img-wrapper{
	white-space:nowrap;
	overflow:hidden;

}
.jia-img-wrapper img{
	margin-right:6px;
}
.jia-seller-info{
	margin:0 18px;
}
.jia-seller-info h1{
    font-size:14px;
	line-height:14px;
	color:rgb(7,17,27);
	margin:18px 0 12px 0;
}
.jia-info{
	padding:16px 12px;
	font-size:12px;
	font-weight:200;
	line-height:16px;
	color:rgb(7,17,27);
	border-bottom:1px solid rgba(7,17,27,0.1);
}
.jia-info:last-child{
	border:none;
}
</style>
