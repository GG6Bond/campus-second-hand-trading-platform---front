<template>
	<view class="content">
		<view class="sign" v-if="!isLogin">
			<button class="sign-in" @click="gotoPage('signIn')">去登录</button>
		</view>
		
		<!-- 轮播图 -->
		<!-- 标签里面不能用插值语法 -->
		<swiper class="swiper" :indicator-dots="true"  :interval="3000" :duration="1000">
			<swiper-item  v-for="(item,index) in scrollData" :key="item.pic_id">
				<view class="swiper-item" @click="getInfo(item.pic_url)">
					<image :src="item.pic_url" mode="scaleToFill"></image>
				</view>
			</swiper-item>
		</swiper>
		

		
		 
		
		
		<!-- <view class="search"> -->
			<!-- <searchBar @searchResult="getResult"></searchBar> -->
			<!-- <search-bar @searchResult="getResult"></search-bar> -->
		<!-- </view> -->
		
		<view class="hint">
			在本平台发布商品请遵守相关法律法规，严禁发布违法信息。
		</view>
		
		<view class="nav">
			<view class="nav-item" v-for="(item,index) in navData" :key="index" @click="gotoClassify(index)">
				<view class="icon" :class="item.class"></view>
				<text>{{item.text}}</text>
			</view>
		</view>
		
		<view class="today">
			<view class="title">
				今天的商品：
			</view>
			<scroll-view scroll-x="true" class="scroll-box">
				<shoppingListItemUpDown :data="todayList"></shoppingListItemUpDown>
			</scroll-view>
		</view>
		
		<view class="today">
			<view class="title">
				推荐商品：
			</view>
			<scroll-view scroll-x="true" class="scroll-box">
				<shoppingListItemUpDown :data="todayList"></shoppingListItemUpDown>
			</scroll-view>
		</view>
		
		<view class="login">
 			<!-- <wx-login></wx-login> -->
		</view>
	</view>
</template>

<script>
	import {
		myRequest
	} from "@/util/api.js"

	export default {
		data() {
			return {
				title: '校园二手交易平台',
				isLogin: false,
				show: true,
				shoppingListItem: [],
				user: {},
				todayList: [],
				searchData: [],
				scrollData:[],
				navData: [{
					text: "生活用品",
					class: "icon-cart"
				}, {
					text: "书本",
					class: "icon-books"
				}, {
					text: "电子产品",
					class: "icon-display"
				}, {
					text: "其它",
					class: "icon-flickr"
				}, ]
			}
		},
		onShow() {
			let url = "../sign/signIn/signIn"
			this.isLogin = this.$checkLogin(url, false);
			this.user = this.$getUser();
			this.getSwiper();
			this.getProductList();
		},
		methods: {
			gotoPage(sign) {
				let url = "../sign/" + sign + "/" + sign
				uni.navigateTo({
					url
				})
			},
			async getProductList() {
				const res = await myRequest({
					url: "/api/getProductList/" + this.user.user_id
				})
				this.shoppingListItem = res.data.message;
				this.getToday();
				// console.log(this.shoppingListItem)
			},
			// 获取今天发布的商品
			getToday() {
				let now = new Date;
				// console.log(new Date(parseInt(now)))
				let mon = now.getMonth();
				let day = now.getDate();
				let todayList = [];
				for (let i of this.shoppingListItem) {
					let date = new Date(parseInt(i.product_id))
					if (date.getMonth() === mon && date.getDate() === day) {
						todayList.push(i)
					}
				}
				this.todayList = todayList;
			},
			getResult(data) {
				let _self = this;
				this.searchData = data;
				uni.navigateTo({
					url: "../searchResult/searchResult",
					success(res) {
						// 通过eventChannel向被打开页面传送数据
						res.eventChannel.emit('acceptDataFromOpenerPage', _self.searchData)
					}
				})
			},
			gotoClassify(index) {
				uni.navigateTo({
					url: "../itemClassify/itemClassify?classify=" + this.navData[index].text
				})
				// console.log(index)
			},
			async getSwiper(){
				const res = await myRequest({
					url: "/api/getSwiper/",
					method:'POST'
				})
				console.log(res.data.message);
				console.log(res.data.message[0].pic_url);
				this.scrollData = res.data.message;
				console.log(this.scrollData);
			},
			
			getInfo(item){
				console.log(item);	
			}
		}
	}
</script>

<style scoped lang="scss">
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;

		.today {
			width: 100%;
			padding: 30rpx 10rpx;

			.title {
				padding: 30rpx;
				font-size: 60rpx;
				text-align: start;
			}
		}

		.logo {
			height: 200rpx;
			width: 200rpx;
			margin-top: 200rpx;
			margin-left: auto;
			margin-right: auto;
			margin-bottom: 50rpx;
		}

		.swiper{
			border: #1b92ff;
			width: 100%;
			height: 200px;
			// height: 100%;
			image{
				// border: 10px solid red;
				width: 100%;
				// height: 100%;
			}
		}
		.hint {
			color: #939393;
			padding: 20rpx;
			line-height: 30px;
		}

		.nav {
			width: 100%;
			display: flex;
			text-align: center;
			height: 100rpx;
			align-items: center;
			padding-top: 50rpx;
			// flex-wrap: wrap;

			.nav-item {
				width: 25%;

				text {
					display: inline-block;
					padding-top: 10rpx;
				}

				.icon {
					color: #1b92ff;
					font-size: 70rpx;
				}

				.icon-cart:before {
					content: "\e90c";
				}

				.icon-books:before {
					content: "\1f4d2";
				}

				.icon-display:before {
					content: "\1f5b3";
				}

				.icon-flickr:before {
					content: "\1f308";
				}
			}
		}

		.scroll-box {
			white-space: nowrap;
		}

		.sign {
			position: fixed;
			// bottom: var(--window-bottom);
			bottom: 60px;
			width: 100%;
			// background-color: #FFFFFF;
			flex-direction: column-reverse;
			z-index: 999;

			.sign-in {
				width: 60%;
				right: 0;
				border: 0px;
				background-color: #1b92ff;
				color: #FFFFFF;
				border-radius: 50px;
			}
		}
	}
</style>
