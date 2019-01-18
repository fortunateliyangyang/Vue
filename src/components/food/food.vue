<template>
   <transition name="move">
   	<div class="food" v-show="showFlag" ref="food">
   		<div class="content">
   			<!-- 顶部图片 -->
   			<div @click="hide" class="image-header">
   				<img :src="food.image">
   				<div class="icon-back">
   					<i class="icon-arrow_lift"></i>
   				</div>
   			</div>
   			<!-- 中间内容-->
   			<div class="content">
   				<h1 class="title">{{food.name}}</h1>
   				<div class="detail">
   					<span class="sell-count">月售{{food.sellCount}}份</span>
   					<span class="rating">好评率{{food.rating}}</span>
   				</div>
   				<div class="price">
   					<span class="now"><span class="sign">￥</span>{{food.price}}</span>
   					<span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
   				</div>
                <div class="cartcontrol-wrapper">
                	<cartcontrol :food="food" @add="addFood"></cartcontrol>
                </div>
               <transition name="fade">
               	<div class="buy" @click.stop.prevent="addFirst" v-show="!food.count||food.count===0">加入购物车</div>
               </transition>  
   			</div>
   			<!-- 分隔栏 -->
   			<split v-show="food.info"></split>
   		    <!-- 商品介绍 -->
   			<div class="info" v-show="food.info">
   				<h1 class="title">商品介绍</h1>
   				<p class="text">{{food.info}}</p>
   			</div>
   			<!-- 分隔栏 -->
   			<split></split>
   			<!-- 商品评价 -->
   			<div class="rating">
   				<h1 class="title">商品评价</h1>
   				<ratingselect :selectType="selectType" 
                              :onlyContent="onlyContent" 
                              :ratings="food.ratings"
                              :desc="desc"
                              @select="selectRating"
                              @toggle="toggleContent">
                              </ratingselect>
   				<div class="rating-wrapper">
   					<ul style="padding: 0">
   						<li v-show="needshow(rating.rateType, rating.text)" v-for="rating in food.ratings" class="rating-item border-1px">
   							<div class="user">
   								<span class="name">{{rating.username}}</span>
   								<img class="avatar" :src="rating.avatar">
   							</div>
   							<div class="time">{{rating.rateTime | formatDate}}</div>
   					        <p class="text"><span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>{{rating.text}}</p>
   						</li>
   					</ul>
   					<div class="no-ratings" v-show="!food.ratings||!food.ratings.length">暂无评价</div>
   				</div>
   			</div>
   		</div>
   	</div>
   </transition>
</template>
<script> 
	import Vue from'vue'
	import BScroll from 'better-scroll';
	import split from '../split/split'
	import {commonFormatDate} from'../../common/js/date'
	import ratingselect from '../ratingselect/ratingselect'
	import cartcontrol from'../cartcontrol/cartcontrol'

	const ALL = 2;

	export default {
		props:{
			food:{
				type:Object
			}
		},
		data(){
			return{
				showFlag:false,
				selectType: ALL,
                onlyContent: false,
                desc: {
                  all: '全部',
                  positive: '推荐',
                  negative: '吐槽'
			    }
	        }
	    },
	    methods:{
	    	show(){
	    		this.showFlag=true;
	    		this.selectType = ALL;    //初始化后再传给ratingselect组件
	    		this.onlyContent = false;

	    		this.$nextTick(() =>{
	    			if(!this.scroll){
	    				this.scroll = new BScroll(this.$refs.food,{
	    					click:true
	    				});
	    			}else{
	    				this.scroll.refresh();
	    			}
	    		})
	    	},
	    	hide(){
	    		this.showFlag=false
	    	},
	    	addFirst(event){
	    		if(!event._constructed){
	    			return
	    		}
	    		this.$emit('add',event.target)
	    		Vue.set(this.food,'count',1)
	    	},
	    	addFood(target){
	    	    this.$emit('add', target);
	    	},
	    	selectRating(type){
	    		this.selectType = type;
	    		this.$nextTick(()=>{
                    this.scroll.refresh();
	    		})
	    	},
	    	toggleContent(){
	    		this.onlyContent = !this.onlyContent
	    		this.$nextTick(()=>{
                    this.scroll.refresh();
	    		})
	    	},
	    	needshow(type,text){
	    		if(this.onlyContent &&!text){  //只查看有内容但是没有评价内容的时候
	    			return false
	    		}else if(this.selectType===ALL){     // 如果是显示全部内容，每条直接返回true
	    			return true
	    		}else{
	    			return type === this.selectType // 返回选择的类型的评价
	    		}
	    	}
	    },
	    filters:{
	        formatDate(time){
	        	let date = new Date(time)
	        	return commonFormatDate(date,'yyyy-MM-dd hh:mm');
	        }
	    },
	    components:{
	    	cartcontrol,
	    	BScroll,
	    	split,
	    	ratingselect,
	    
	    }
    }
</script>


<style lang="stylus" rel="stylesheet/stylus">
 @import "../../common/stylus/mixin.styl"
 .food{
 	position:fixed
 	left:0
 	top:0
 	bottom:48px
 	z-index:30
 	width:100%
 	background:#fff
 	&.move-enter-active,&.move-leave-active{
 		transition:all 0.2s linear;
 	}
 	&.move-enter,&.move-leave-active{
 		transform:translate3d(100%,0,0);
 	}
 	.image-header{
 		position:relative
 		top:0
 		left:0
 		width:100%
 		heitght:0
 		padding-top:100%  //相对于盒子宽度 %，是个正方形，当图片还没加载好的时候留出位置，避免塌陷
 		img{
 			position:absolute
 			top:0
 			left:0
 			width:100%
 			height:100%
 		}
 		.icon-back{
 			position:absolute
 			left:0
 			top:20px
 			.icon-arrow_lift{
 				display:block
 				padding:10px
 				font-size:20px
 				color:#fff
 				font-weight:700
 			}
 		}
 		
 	}
 	.content{
 		position:relative
 		padding:8px
 		.title{
 			font-size:14px
 			line-height:14px
 			margin-bottom:18px
 		}
 		.detail{
 			font-size:10px
 			line-height:10px
 			margin-bottom:18px
 			color:rgb(147,153,159)
 			.sell-count {
              margin-right: 12px;
            }
 		}
 		.price{
 			margin-bottom:18px
 			line-height:14px	
 			.now{
 				font-size:14px
 				color:rgb(240,20,20)
 				font-weight:700
 				margin-right:8px 	
 				line-height:14px			
 			}
 			.old{
 				font-size:10px
 				color:rgb(147,153,159)
 				text-decoration:line-through
 				line-height:10px	
 			}
 		}
 		.cartcontrol-wrapper{
 		    bottom: 12px;
            position: absolute;
            right: 12px;
 		}
 		.buy{
 		    position:absolute
 		    right:18px
 		    bottom:18px
 		    line-height:12px
 		    font-size:10px
 		    padding:6px 12px
 		    color:rgb(255,255,255)
 		    border-radius:24px
 		    z-index:100
 		    background: rgb(0, 160, 220);
            opacity: 1;
            &.fade-enter-active, &.fade-leave-active {
                transition: all .2s;
            }
            &.fade-enter, &.fade-leave-to {
              opacity: 0;
              z-index: -1;
            }
 		}                                     
 	}
 	.info{
 		padding:18px
 		.title{
 			line-height: 14px;
            margin-bottom: 6px;
            font-size: 14px;
            color: rgb(7, 17, 27);
 		}
 		.text{
 			margin-left:8px
 			line-height:24px
 			font-size:12px
 			font-weight:200
 			color:rgb(147,153,159)
 		}
 	}
 	.rating{
 		padding:18px
 		.title{
 			line-height:14px
 			font-size:14px
 			color: rgb(7, 17, 27);
 			margin-bottom:0
 		}
 		.rating-wrapper{
 			padding:0 18px
 			.rating-item{
 				position:relative
 				padding:16px 0
 				list-style:none
 				.user{
 					position:absolute
 					right:0
 					line-height:12px
 					font-size:10px
 					.name{
 						display:inline-block
 						margin-right:6px
 						vertical-align:top
 					}
 					.avatar {
 						display:inline-block
                        border-radius: 50%;
                        width:12px
                        height:12px
                    }
 				}
 				.time {
                  margin-bottom: 6px;
                  line-height: 12px;
                  font-size: 10px;
                  color: rgb(147, 153, 159);
                }
                .text {
                   line-height: 16px;
                   font-size: 12px;
                   color: rgb(7, 17, 27);
                    .icon-thumb_up, .icon-thumb_down {
                     margin-right: 4px;
                     line-height: 16px;
                     font-size: 12px;
                    }
                    .icon-thumb_up{
                    	color: rgb(0, 160, 220);
                    }
                    .icon-thumb_down{
                    	color: rgb(147, 153, 159);
                    }
 			    }
 		    }
 		    .no-ratings{
 		    	padding: 16px 0;
                font-size: 12px;
                color: rgb(147, 153, 159);
 		    }
 	    }
    }
   }
</style>