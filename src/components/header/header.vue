<template>
 <div class="header">

 	<div class="header-wrapper">
 		<div class="header-avatar">
          <img :src="seller.avatar" width="64" height="64" />
        </div>
        <div class="header-content">
        	<div class='header-title'>
             <span class="header-brand"></span>
             <span class="header-name">{{seller.name}}</span></div>
             <div class="header-description">{{seller.description}}/{{seller.deliveryTime}}分钟送达</div>
             <div v-if="seller.supports" class="header-support">
             <span class="header-icon"></span>
             <span class="header-text">{{seller.supports[0].description}}</span>
             </div>       	
        </div>
        <div v-if="seller.supports" class="header-support-count" @click="showDetail">
            <span class="header-count">{{seller.supports.length}}个</span><i class="icon-keyboard_arrow"></i>
        </div>
    </div>
 	<div class="header-bulletin-wrapper" @click="showDetail">
      <span class="header-bulletin-title"></span><span class="header-bulletin-text">{{seller.bulletin}}</span><i class="icon-keyboard_arrow_right"></i>
    </div>

    <div class="header-background">
        <img :src="seller.avatar" width="100%" height="100%">
    </div>

    <div v-show="detailShow" class="header-detial">
        <div class="header-detail-wrapper clearfix">
            <div class="header-detial-main">
               <h1 class="header-name">{{seller.name}}</h1> 
                <div class="header-star-wrapper">
                    <star :size='48' :score='seller.score'></star>
                </div>
                <div class="header-main-title">
                    <div class="header-line"></div>
                    <div class="header-text">优惠信息</div>
                    <div class="header-line"></div>
                </div>
                <ul v-if="seller.supports" class="header-supports">
                  <li class="header-support-item" v-for="item in seller.supports">
                   <span class="header-support-icon" :class="iconClassMap[item.type]"></span>
                   <span class="header-support-text">{{item.description}}</span>
                 </li>
               </ul>
               <div class="header-main-title">
                    <div class="header-line"></div>
                    <div class="header-main-text">商家信息</div>
                    <div class="header-line"></div>
                </div>
                <div class="header-bulletin-text">{{seller.bulletin}}</div>


            </div>
        </div>
        <div class="header-detial-close" @click="hiddenDetail">
            <i class="header-icon-close"><strong>×</strong></i>
        </div>
    </div>
 	

</div>
</template>

<script>
import star from 'components/star/star.vue'

export default {

	props: {
    seller: {
      type: Object
    }
  },
  created() {
    this.iconClassMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee1']
  },
  data(){
    return{
       detailShow:false
       
    }
  },
  methods:{
    showDetail(){
        this.detailShow = true
    },
    hiddenDetail(){
        this.detailShow=false
    }
  },
  components: {
    star
  }
  


}
  
</script>

<style>

.header{
    color:#fff;
    position:relative;
    background:rgba(14,27,47,0.5);
    overflow:hidden;

}
.header-wrapper{
    padding:24px 12px 18px 24px;
    font-size:0;
    position:relative;
}
.header-avatar{
    display:inline-block;
    vertical-align:top; 
}
.header-avatar img{
    border-radius:5px;
}
.header-content{
    display:inline-block;
    margin-left:16px;
    font-size:14px;
}
.header-title{
    margin:2px 0px 8px 0px;
}
.header-brand{
    width:30px;
    height:18px;
    display:inline-block;
    background-image: url('brand@3x.png');
    background-size:30px 18px;
    background-repeact:no-repeact; 
    vertical-align:top;  
}
.header-name{
    margin-left:6px;
    font-size:16px;
    line-height:bold;
}
.header-description{
    margin-bottom:10px;
    line-height:12px;
    font-size:12px;
}
.header-icon{
    display:inline-block;
    
    width:12px;
    height:12px;
    margin-right:4px;
    background-size:12px 12px;
    background-repeact:no-repeact;
    background-image: url('decrease_1@3x.png');

}
.header-text{
    vertical-align:top;
    line-height:12px;
    font-size:10px;
    font-weight:200;
    color:rgb(250,250,250)
    }

.header-support-count{  
    position:absolute;
    right:12px;
    bottom:18px;
    padding:0px 8px;
    width:36px;
    height:24px;
    border-radius:14px;
    background:rgba(0,0,0,0.2);
    
}
.header-count{
    font-size:10px;
    display:inline-block;
    margin-top:6px;
    
}
.icon-keyboard_arrow{
    display:inline-block;
    width: 5px;
    height: 5px;
    border-top: 2px solid #ffffff;
    border-right: 2px solid #ffffff;
    transform: rotate(45deg);
    margin-left:4px;
}
.header-bulletin-wrapper{
    position:relative;
    height:28px;
    line-height:28px;
    padding:0 22px 0 12px;
    white-space:nowrap;
    overflow:hidden;
    text-overflow:ellipsis;
    background:rgba(7,17,27,0.2);
    
}
.header-bulletin-title{
    background-image:url('bulletin@3x.png');
    display:inline-block;
    width:22px;
    height:12px;
    margin-top:8px;
    vertical-align:top;
    background-size:22px 12px;
    background-repect:no-repeact;
}
.header-bulletin-text{
    vertical-align:top;
    font-size:10px;
    margin:0 4px;
}
.header-background{
    position:absolute;
    top:0px;
    left:0px;
    width:100%;
    height:100%;
    z-index:-1;
    filter: blur(2px);   
}
.icon-keyboard_arrow_right{
    width: 5px;
    height: 5px;
    border-top: 2px solid #ffffff;
    border-right: 2px solid #ffffff;
    transform: rotate(45deg);
    position:absolute;
    right:12px;
    top:10px;
}
.header-detial{
    position:fixed;
    z-index:100;
    top:0px;
    left:0px;
    width:100%;
    height:100%;
    overflow:hidden;
    background:rgba(7,17,27,0.8)
}
.header-detail-wrapper{
    min-height:100%;
    width:100%;
}
.header-detial-main{
    margin-top:64px;
    padding-bottom:64px;

}
.header-main-name{
    line-height:16px;
    text-align:center;
    font-size:16px;
    font-weight:700;
}
.header-detial-close{
    position:relative;
    width:32px;
    height:32px;
    margin:-64px auto 0 auto;
    text-align:center;
    font-size:32px


}
.header-icon-close{
    display:block;
    width:18px;
    background-color:none;
    font-size:18px;
    color:#999;
    border:1px solid #999;
    border-radius:20px;
    text-align:center;  
}
.header-star-wrapper{
    margin-top:18px;
    padding:2px 0;
    text-align:center;
}
.header-main-title{
    display:flex;
    width:80%;
    margin:28px auto 24px auto;
}
.header-line{
    flex:1;
    position:relative;
    top:-6px;
    border-bottom:1px solid rgba(255,255,255,0.2)
}
.header-text{
    padding:0 12px;
    font-size:12px;
    font-weight:700;
}
.header-supports{
padding: 0 0 28px 36px;
}
.header-support-item{
    color: white;
    padding: 0 6px 12px 16px;

}
.header-support-icon{
  display: inline-block;
  vertical-align: top;
  width: 16px;
  height: 16px;
  margin-right: 6px;
  background-size: 100% 100%;
  background-repeat: no-repeat;
}
.decrease{
    background-image:url('decrease_2@3x.png');
}

.discount{
    background-image:url('discount_2@3x.png');
}
.invoice{
    background-image:url('invoice_2@3x.png');
}
.guarantee{
    background-image:url('guarantee_2@3x.png');
}
.special{
    background-image:url('special_2@3x.png');
}
.header-support-text{
    vertical-align: middle;
    font-size: 12px;
    font-weight: 200;
    color: rgb(255,255,255);
    line-height: 12px;
}
.header-bulletin-text{
    width:80%;
    padding:0 48px;
    font-size: 12px;
    font-weight: 200;
    color:rgb(255,255,255);
    line-height: 24px;          
}

</style>