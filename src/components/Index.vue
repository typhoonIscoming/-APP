<template>

	<div class="page">
		<message ref="message"></message>
		<div class="head">
			<div style="width: 100%;height: 0.4rem;display: flex;align-items: center;justify-content: space-around;">
			<div class="logo">
				<span style="background: url(../../static/img/system/logo.jpg)no-repeat center center;"></span>
			</div>
			<input class="search" placeholder="搜索目的地/游记" />
			</div>
		</div>
		<transition-Group :name="slidename" class="contentBox">
			<div key=1 style="width: 100%;height: 200px;">
				
				<swiper :options="swiperOption" ref="mySwiper" >
				    <swiper-slide>I'm Slide 1</swiper-slide>
				    <swiper-slide>I'm Slide 2</swiper-slide>
				    <swiper-slide>I'm Slide 3</swiper-slide>
				    <swiper-slide>I'm Slide 4</swiper-slide>
				    <swiper-slide>I'm Slide 5</swiper-slide>
				    <swiper-slide>I'm Slide 6</swiper-slide>
				    <swiper-slide>I'm Slide 7</swiper-slide>
				    <div class="swiper-pagination"  slot="pagination"></div>
				    <!--<div class="swiper-button-prev" slot="button-prev"></div>
				    <div class="swiper-button-next" slot="button-next"></div>-->
				    <div class="swiper-scrollbar"   slot="scrollbar"></div>
			  	</swiper>
				
			</div>
			
			<div class="container" v-show="mainarea" key=2>
				<div class="swiper-container" v-show="false">
					<div class="swiper-wrapper">
						<div class="swiper-slide" v-for="bannerItem in bannerList">
							<img :src="bannerItem.img" />
						</div>
					</div>
					<div class="swiper-pagination"></div>
				</div>

				<div class="content" v-cloak>
					<div v-for="(productItem,productIndex) in productList" class="floorItem">

						<div class="productTop flex-between" @click="onCategory(productIndex)">
							<p class="productTop-text">{{productItem.Category.TopText}}</p>
							<div class="flex-align-center">
								<p class="productTop-text">{{productIndex+1}}F</p>
								<img src="../../static/img/icon/arrowBack.png" class="arrowImg" />
							</div>
						</div>
						<div class="productContent">
							<div class="productBox flex" ref='div'>
								<div class="productItem" v-for="goodsItem in productItem.SalesProduct">
									<div class="itemBox">
										<div @click="onGoodsDetail(goodsItem,goodsItem.CategoryId)">
											<img v-lazy="goodsItem.GoodsImage" class="itemImg" />
											<div>
												<p class="goods-name text-ellipsis">{{goodsItem.GoodsName}}</p>
											</div>
										</div>

										<div class="addCartBox flex-between">
											<p><span class="goods-price">¥ {{goodsItem.GoodsPrice}}</span></p>
											<img src="../../static/img/icon/shop_addCart.png" @click="onAddCart(goodsItem,goodsItem.GoodsName)" v-show="!goodsItem.shopAddCart" />
											<img src="../../static/img/icon/shop_addCart_select.png" @click="onAddCart(goodsItem.GoodsName)" v-show="goodsItem.shopAddCart" />
										</div>

									</div>

								</div>
							</div>
						</div>
					</div>

				</div>

			</div>
		</transition-Group>
		<footers :urlRouter="$route.path" :cartnum='cartLength' ref="footer"></footers>
	</div>

</template>

<script>
	import Headers from './base/Header.vue';
	import Footers from './base/Footer.vue';
	import Message from './base/Message.vue';
	import '../../static/css/swiper.min.css';
	import Swiper from '../../static/js/swiper.min';
	
	import { swiper, swiperSlide } from 'vue-awesome-swiper'
	
	import { mapGetters, mapMutations } from 'vuex';
	import Vue from 'vue'
	import { Search } from 'vux'
//	Vue.use(swiper)
	export default {
		
		data() {
			return {
				bannerList: [],
				productList: [],
				cartLength: 0,
				slidename: 'slide-back',
				mainarea: false,
				value:"输入商品",
				results:[],
				swiperOption: {//以下配置不懂的，可以去swiper官网看api，链接http://www.swiper.com.cn/api/
          // notNextTick是一个组件自有属性，如果notNextTick设置为true，组件则不会通过NextTick来实例化swiper，也就意味着你可以在第一时间获取到swiper对象，<br>假如你需要刚加载遍使用获取swiper对象来做什么事，那么这个属性一定要是true
          			direction: 'horizontal',//vertical
		          	notNextTick: true,
		          // swiper configs 所有的配置同swiper官方api配置
		          	autoplay: 1000,
		          	grabCursor : true,
		          	setWrapperSize :true,
		          	autoHeight: true,
		          	pagination : '.swiper-pagination',
		          	paginationClickable :true,
//		          	prevButton:'.swiper-button-prev',//上一张
//		          	nextButton:'.swiper-button-next',//下一张
		          	scrollbar:'.swiper-scrollbar',//滚动条
		          	mousewheelControl : true,
		          	observeParents:true,
		          	debugger: true // 如果自行设计了插件，那么插件的一些配置相关参数，也应该出现在这个对象中，如下debugger
		        }
			}
		},
		components: {
			Headers,
			Footers,
			Message,
			Search,
			swiper,
			swiperSlide
		},
		computed: {
			...mapGetters([
				'this.$store.state.carts',
				'this.$store.state.comname'
			])
		},
		mounted() {
			var that=this;
			this.mainarea = true;
			
			const swiper = new Swiper('.swiper-container', {
				pagination: '.swiper-pagination',
				paginationClickable: true,
				spaceBetween: 0,
				autoplay: 3000,
//					effect: 'fade',
				
				loop:true,
//					direction: 'horizontal',//vertical
//		          	notNextTick: true,
//		          	autoplay: 3000,
//		          	grabCursor : true,
//		          	setWrapperSize :true,
//		          	autoHeight: true,
//		          	pagination : '.swiper-pagination',
//		          	paginationClickable :true,
////		          	prevButton:'.swiper-button-prev',//上一张
////		          	nextButton:'.swiper-button-next',//下一张
//		          	scrollbar:'.swiper-scrollbar',//滚动条
//		          	mousewheelControl : true,
//		          	observeParents:true,
//		          // 如果自行设计了插件，那么插件的一些配置相关参数，也应该出现在这个对象中，如下debugger
//		          	debugger: true,
			})

			this.getGoodsList();
			this.getBannerList();
			/*判断动画是进还是出*/
			if(this.$store.state.comname == 'category' || 'cart' || 'member') {
				this.slidename = 'slide-back';
			} else {
				this.slidename = 'slide-go'
			}
			this.setComname('index');
		},
		methods: {
			
			setFocus () {
		      	this.$refs.search.setFocus()
		    },
		    resultClick (item) {
		      	window.alert('you click the result item: ' + JSON.stringify(item))
		    },
		    getResult (val) {
		      	console.log('on-change', val)
		      	this.results = val ? getResult(this.value) : []
		    },
		    onSubmit () {
		      	this.$refs.search.setBlur()
		      	this.$vux.toast.show({
			        type: 'text',
			        position: 'top',
			        text: 'on submit'
		      	})
		    },
		    onFocus () {
		      	console.log('on focus')
		    },
		    onCancel () {
		      	console.log('on cancel')
		    },
			
			/*header子组件向父组件传值*/
			dothing(data){
				console.log(data?data:"没有值！");
			},
			/*获取商品列表*/
			getGoodsList() {
				const that = this;
				this.$http.get('/api/homedata').then(function(res) {
					that.productList = res.data.data;
				}).catch(function(error) {
					console.log(error);
				});
			},
			/*获取轮播列表*/
			getBannerList() {
				const that = this;
				this.$http.get('/api/bannerdata').then(function(res) {
						that.bannerList = res.data.data;
					})
					.catch(function(error) {
						console.log(error);
					});
			},
			/*进入商品详情*/
			onGoodsDetail(item, id) {
				this.$router.push({
					path: '/detail',
					query: {
						id: id
					}
				});
				this.setGoods(item)
			},
			/*添加到购物车*/
			onAddCart(item, name) {
				
				if(!this.$store.state.carts.includes(item)) {
					if(!this.$refs.footer.showNum) {
						this.cartLength = this.$store.state.carts.length + 1;
						this.$refs.footer.showNum = true;
						this.setCarts(item);
					}
				} else {
					this.$refs.message.messageShow = true;
				}

			},
			/*前往分类页面*/
			onCategory(index) {
				this.setTabindex(index);
				this.$router.push('./category');
			},

			...mapMutations({
				setGoods: 'SET_GOODS',
				setCarts: 'SET_CARTS',
				setTabindex: 'SET_TABINDEX',
				setComname: 'SET_COMNAME'
			})

		}

	}
</script>

<style lang="less" scoped>
	@import '../../static/less/variable.less';
	.swiper-slide img {
		width: 100%;
		height: 100%;
	}
	.container{
		padding-bottom: 0;
		/*padding: 0;*/
	}
	.productTop {
		padding: 0 .2rem;
		height: .6rem;
		line-height: .6rem;
		border-bottom: @base_boder;
		border-top: @base_boder;
		margin-bottom: .2rem;
	}

	.productTop-text {
		font-size: .28rem;
	}

	.arrowImg {
		width: .4rem;
		height: .4rem;
	}
	.floorItem:nth-last-child(1){
		margin-bottom: .88rem;
	}
	.productItem {
		width: 3.8rem;
		box-sizing: border-box;
		margin-bottom: .2rem;
		padding: 0 .2rem;
		flex:1;
		&:nth-child(odd) {
			margin-right: 2%;
			border-right: 1px solid #ccc;
		}
	}

	.itemImg {
		width: 100%;
		height: 100%;
	}

	.addCartBox {
		img {
			width: .4rem;
			height: .4rem;
			padding-right: .2rem;
		}
	}
	.contentBox{
		width: 100%;
		margin-top: 0.8rem;
	}
	
	.page{
		position: relative;
	}
	
	.head{
		width: 100%;
		height: 0.8rem;
		background: #e95c53;
		display: flex;
		align-items: center;
		position: fixed;
		left: 0;
		top: 0;
		z-index: 99;
	}
	.logo{
		width: 20%;
		height: 0.7rem;
	}
	.logo>span{
		display: inline-block;
		width: 100%;
		height: 0.7rem;
	}
	.search{
		width: 4.5rem;
		padding-left: 5px;
		height: 0.5rem;
		border-radius: 5px;
		background: #d5372c;
		border: 0;
		outline: none;
		font-size: 0.2rem;
		color: white;
		text-align: center;
	}
	.search::-webkit-input-placeholder{
	    color: white;
	}
	.swiper-container {
	    width: 100%;
	    height: 100%;
	    div{
	    	div{
	    		height: 375px;
	    	}
	    }
	}
	.swiper-slide{
		background: white;
		text-align: center;
	}
</style>
