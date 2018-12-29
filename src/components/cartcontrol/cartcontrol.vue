<template>
   <div class="cartcontrol">
  
      <div class="cart-decrease" @click.stop.prevent="deleteCart" v-show="food.count>0">
          <i class="icon-offline"></i>
      </div>
   
    <div class="cart-count" v-show="food.count>0">
      {{food.count}}
    </div>
    <div class="cart-add" @click.stop.prevent="addCart">
      <i class="icon-addition_fill"></i>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
export default {
	 props: {
     food:Object
    },
    methods:{
    	 addCart(event){
    	 	if(!event._constructed){
    	 		return
    	 	}   	
    	 	if(!this.food.count){
    	 		Vue.set(this.food,"count", 1)
    	 	}else{
    	 		this.food.count++;
    	 	}
    	 },
    	 deleteCart(){
    	 	if(!event._constructed){
    	 		return
    	 	}
    	 	if(this.food.count){
              this.food.count--;
    	 	}
    	 }

    }
}
</script>

<style>

.cart-decrease{
    display:inline-block;
    padding:6px;
    font-size:24px;
    line-height:24px;
    color:rgb(0,160,220);
    vertical-align:top;

}
.cart-count{
   display:inline-block;
   line-height:24px;
   padding:7px 0;
   text-align:center;
   font-size:10px;
   color: rgb(147,153,159);
}
.cart-add{
    display:inline-block;   
    font-size:24px;
    line-height:24px;
    color:rgb(0,160,220);
    padding:6px;
    vertical-align:top;

}

</style>
