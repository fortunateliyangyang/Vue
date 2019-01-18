<template>
	<div class="ratings" ref="ratings">
		<div class="ratings-content">
			<!-- 综合评分 -->
			<div class="overview">
				<div class="overview-left">
					<h1>{{seller.score}}</h1>
				    <span class="title">综合评分</span>
				    <span class="rank">高于周边商家{{seller.rankRate}}%</span>
				</div>
				<div class="overview-right">
					<div class="score-wrapper">
						<span class="title">服务态度</span>
						<star :size="36" :score="seller.serviceScore"></star>
						<span class="score">{{seller.serviceScore}}</span>
					</div><div class="score-wrapper">
						<span class="title">商品评价</span>
						<star :size="36" :score="seller.serviceScore"></star>
						<span class="score">{{seller.foodScore}}</span>
					</div><div class="score-wrapper">
						<span class="title">送达时间</span>
						<span class="delivery">{{seller.deliveryTime}}分钟</span>
					</div>

				</div>
			</div>
			<!-- 分隔栏 -->
			<split></split>
			<!-- 选择查看的评价 -->
			<div class="selectRating">
				<ratingselect @select="selectRating"
				              @toggle="toggleContent" 
				              :selectType="selectType" 
				              :onlyContent="onlyContent"
				              :ratings="ratings">				    
				</ratingselect>
   				<div class="rating-wrapper">
   					<ul style="padding: 0">
   						<li v-show="needShow(rating.rateType, rating.text)" v-for="rating in ratings" class="rating-item">
                            <div class="avatar">
                              <img :src="rating.avatar" width="28" height="28" alt="">
                            </div>
                            <div class="content">
                              <h1 class="name">{{rating.username}}</h1>
                              <div class="star-wrapper">
                                <star :size="24" :score="rating.score"></star>
                                <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
                              </div>
                              <p class="text">{{rating.text}}</p>
                              <div class="recommend" v-show="rating.recommend && rating.recommend.length">
                                <span class="icon-thumb" :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>
                                <span class="item" v-for="item in  rating.recommend">{{item}}</span>
                              </div>
                              <div class="time">{{rating.rateTime | formatDate}}</div>
                            </div>
                        </li>
   					</ul>
   				</div>
			</div>
			<div>				
			</div>
		</div>
	</div>
</template>
<script >
	import split from '../split/split'  
	import star from '../star/star'
	import ratingselect from '../ratingselect/ratingselect'
	import BScroll from 'better-scroll';
	import {commonFormatDate} from '../../common/js/date'

    const ALL = 2
    const ERR_OK = 0;
	export default{
		props:{
			seller:{
				type:Object
			}
		},
		data(){
			return{
				ratings:[],
				selectType:ALL,
				onlyContent:false
			}
		},
		created(){
			const url = '/api/ratings'
			this.$http.get(url).then((response)=>{
				response = response.data;
				if(response.errno ===ERR_OK){
					this.ratings =response.data
					this.$nextTick(()=>{
						this.scroll = new BScroll(this.$refs.ratings,{
							click:true
						})
				    })
			    }
		    })
		},
		methods: {
			selectRating(type){
				this.selectType = type;
				this.$nextTick(()=>{
					this.scroll.refresh()		
				})
			},
			toggleContent(){
				this.onlyContent = !this.onlyContent
				this.$nextTick(()=>{
					this.scroll.refresh()
				})
			},
			needShow(type,text){
				if(this.onlyContent&&!text){
					return false
				}else if(this.selectType===ALL){
					return true
				}else{
					return type === this.selectType
				}
			}
		},
		filters:{
			formatDate(time){
				let date = new Date(time)
				return commonFormatDate(date,'yyyy-MM-dd hh:mm')
			}
		},
		components:{
			split,
			star,
			ratingselect,
			commonFormatDate
		}
	}
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin.styl"
	.ratings{
		position: absolute;
        top: 174px;
        bottom: 0;
        left: 0;
        overflow:hidden
        width: 100%;
        
		.ratings-content{
			.overview{
				display:flex
				padding:18px 0
				.overview-left{
					flex:0 0 137px
					text-align:center
					border-right: 1px solid rgba(7, 17, 27, 0.1);
					@media only screen and (max-width: 320px) {
                      flex: 0 0 120px;
                      width: 120px;
                    }
					span{
						display:block
					}
					h1{
						font-size:24px
						color:rgb(255,153,0)
						line-height:28px
						margin-bottom:6px
					}
					.title{
						margin-bottom:8px
						font-size:12px
						color:rgb(7,17,27)
						line-height:12px
						font-weight:700
					}
					.rank{
						font-size:10px
						color:rgb(147,153,159)
						line-height:10px
					}
				}
				.overview-right{
					flex:0 0 237px
					padding:18px 0
				    @media only screen and (max-width: 320px) {
                      padding-left: 6px;
                    }
					.score-wrapper{
					   	padding:0 24px 8px 24px
					   	.title{
					   		display: inline-block;
					   		font-weight:700
                            line-height: 18px;
                            vertical-align: top;
                            font-size: 12px;
                            color: rgb(7, 17, 27);   
					   	}
					   	.star{
                            display: inline-block;
                            padding: 0 6px;
                            line-height:18px
                            vertical-align: center;
                           
					   	}
					   	.score {
                            display: inline-block;
                            line-height: 18px;
                            vertical-align: top;
                            font-size: 12px;
                            color: rgb(255, 153, 0);
                        }
                        .delivery{
                        	margin-left:12px
                        	display: inline-block;
                            vertical-align: center;
                            font-size: 10px;
                            color: rgb(147, 153, 159);
                        }
					}
				}
			}
			.selectRating{
				padding:18px
				.rating-wrapper{
					padding:0 18px
					.rating-item{
						list-style:none
						display:flex
						padding:18px 0 0 0
						border-1px(rgba(7, 17, 27, 0.1))
						.avatar{
							flex: 0 0 28px
						    padding-right:12px
						    img {
						    	border-radius:50%
						    }
						}
						.content{
							padding:0 18px 18px 18px
							flex:1
							.name{
								margin:0 0 4px 0
								line-height: 12px;
                                font-size: 10px;
                                color: rgb(7, 17, 27);
							}
							.star-wrapper{
								margin-bottom:6px
								.star {
                                  display: inline-block;
                                  vertical-align:top;
                                  margin-right: 6px;
                                }
                                .delivery{
                                	display: inline-block;
                                    vertical-align:center;
                                    font-size: 10px;
                                    color: rgb(147, 153, 159);
                                }
							}
							.text{
								margin-bottom: 8px;
                                line-height: 18px;
                                color: rgb(7, 17, 27);
                                font-size: 12px;
							}
							.recommend{
								line-height:16px
								.icon-thumb, .item{
									display:inline-block
									margin-right:8px
									font-size: 9px;
								}
								.icon-thumb_up {
                                  color: rgb(0, 160, 220);
                                }
                                .item{
                                	border: 1px solid rgba(7, 17, 27, 0.1);
                                	padding:0 6px
                                	color:rgb(147, 153, 159);
                                	border-radius: 1px;
                                	background: #fff;
                                }
							}
							.time{
								position: absolute;
                                top: 18px;
                                right: 0;
                                line-height: 12px;
                                font-size: 10px;
                                color: rgb(147, 153, 159);
			                }
						}
					}
				}
			}	
		}
    }	
</style>