<template>
   <div class="ratingselect">
   	<div class="ratingselect-type">
   		<span @click="select(2,$event)" class="block positive" :class="{'act':selectType===2}">{{desc.all}}<span class="ratingselect-count">{{ratings.length}}</span></span>
   		<span @click="select(0,$event)" class="block positive" :class="{'acti':selectType===0}">{{desc.positive}}<span class="ratingselect-count">{{positives.length}}</span></span>
   		<span @click="select(1,$event)" class="block negative" :class="{'ac':selectType===1}">{{desc.negative}}<span class="ratingselect-count">{{negatives.length}}</span></span>
   	</div>
   	<div class="switch">
   		<span class="icon-stealth"></span>
   		<span class="switch-text">只看有内容的评价</span>
   	</div>
   </div>
</template>

<script>
const POSITIVE = 0;
const NEGATIVE = 1;
const ALL = 2;
export default {
  props:{
  	ratings:{
  		type:Array,
  		default(){
  			return [];
  		}
  	},
  	selectType:{
       type:Number,
       default:ALL
  	},
  	onlyContent:{
  		type:Boolean,
  		default:false
  	},
  	desc: {
  		type: Object,
  		default(){
  			return{
  				all: "全部",
  				positive: '满意',
  				negative: '不满意'

  			}
  		}
  	}	
  },
  methods:{
  		select(type,event){
  			if(!event._constructed){
  				return
  			}
  			this.selectType=type;
  			this.$emit('ratingtype.select','type');
  		}
  	},
  	computed:{
  		positives(){
  			return this.ratings.filter((rating) => {
  				return rating.rateType ===POSITIVE; 
  			})
  		},
  		negatives(){
  			return this.ratings.filter((rating) => {
  				return rating.rateType ===NEGATIVE
  			})
  		}
  	}
}
</script>

<style>
.ratingselect{

}
.ratingselect-type{
	padding:18px 0;
	margin:0 18px;
	font-size:0px;
}
.block{
	display:inline-block;
	padding:8px 12px;
	margin-right:8px;
	border-radius:1px;
	font-size:12px;
	color:rgb(77,85,95)
}

.ratingselect-count{
    font-size:8px;
    margin-left:2px;
    line-height:16px;
}
.positive{
 background:rgb(0,160,220,0.2);

}
.act{
	 background:rgb(0,160,220);
	 color:#fff;
}
.negative{
	background:rgba(77,85,93,0.2);
}
.acti{
	background:rgb(0,160,220);
	 color:#fff;
}
.ac{
	background:rgb(77,85,93);
	color:#fff;
}
.switch{
 padding:12px 18px;
 line-height:24px;
 border-bottom:1px solid rgba(7,17,27,0.1);
 color:rgb(147,153,159);
 font-size:0;
}
.switch-text{
   font-size:12px;
   display:inline-block;
}
.icon-stealth{
	margin-right:4px;
	font-size:24px;
	display:inline-block;
}

</style>
