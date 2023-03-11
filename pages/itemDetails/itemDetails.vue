<template>
	<view class="itemDetails">
		<!-- 轮播图 start -->
		<swiper class="swiper" :indicator-dots="indicatorDots">
			<swiper-item v-for="(item,index) in imgArr" :key="index">
				<image class="swiper-item" :src="item" @click="previewImg(index)" mode="aspectFit"></image>
			</swiper-item>
		</swiper>
		<!-- 轮播图 end -->
		<!-- 卖家信息 start -->
		<view class="owner">
			<uni-icons class="head" type="person-filled" size="45" color=""></uni-icons>
			<view class="owner-name">{{ownerName}}</view>
			<view class="discrict">
				{{discrictArr[discrict]}}
			</view>
		</view>
		<!-- 卖家信息 end -->

		<!-- 步骤 start -->
		<!-- https://uniapp.dcloud.net.cn/component/uniui/uni-steps.html# -->
		<view class="steps" v-if="status != 0">
			<uni-steps :options="options" :active="status"></uni-steps>
		</view>
		<!-- 步骤 end -->
		
		<view class="box">
			<view class="name">{{title}}
				<span class="price-num">￥{{price}}</span>
			</view>
			
			<view class="tip">
				详细信息：
			</view>
			
			<view class="detail">
				<view>{{detail}}</view>
			</view>
		</view>

		<view class="buttom" v-if="status != 2"></view>

		<view class="buttom-line" v-if="status != 2">

			<!-- 管理员 start -->
			<template v-if="user.user_role < 2">
				<button type="default" class="edit" @click="edit">编辑</button>
			</template>
			<!-- 管理员 end -->
			<template v-else>
				<!-- 关注按钮 start -->
				<button disabled="false" class="follow" v-if="!isLogin">
					<!-- <span class="icon-ban"></span> -->
					<image src="../../static/itemdetail/ban.png" mode=""></image>
					关注
				</button>
				<button class="follow" @click="addFollow" v-else-if="status == 0 && user.user_id != owner">
					<!-- <span :class="isFollow ? 'icon-favorite' : 'icon-not-favorite'"></span> -->
					<image :src="!isFollow ? '../../static/itemdetail/like.png' : '../../static/itemdetail/like-h.png'"
					 mode=""></image>
					{{isFollow ? " 已关注" : " 关注"}}
				</button>
				<!-- 关注按钮 end -->

				<!-- 卖家 start -->
				<button type="default" class="edit" @click="edit" v-if="user.user_id === owner">编辑</button>
				<button type="default" class="edit" v-if="user.user_id == owner && status == 1"
					@click="changeStatus(2)">完成交易</button>
				<button type="default" class="edit" v-if="user.user_id == owner && status == 1"
					@click="changeStatus(0)">终止交易</button>
				<!-- 卖家 end -->

				<!-- 买家 start -->
				<button type="default" :disabled="!isLogin" class="buy" @click="buy"
					v-if="owner !== user.user_id && status === 0">
					<!-- <span :class="isLogin ? '' : 'icon-ban'"></span> -->
					<image :src="isLogin? '':'../../static/itemdetail/ban.png'"  
					v-if="!isLogin"
					mode=""></image>
					购买
				</button>
				<!-- 买家 end -->
			</template>
		</view>
	</view>
</template>

<script>
	import {
		myRequest
	} from "@/util/api.js"

	export default {
		name: "itemDetails",
		data() {
			return {
				indicatorDots: true,
				id: -1,
				imgArr: [],
				price: 0,
				title: "",
				detail: "",
				isLogin: false,
				product: {},
				user: {},
				owner: -1,
				ownerName: "",
				status: -1,
				baseUrl: "",
				isFollow: false,
				discrict: 0,
				discrictArr: ["东校区", "西校区"],
				options: [{
					title: "待出售"
				}, {
					title: "交易中"
				}, {
					title: "交易完成"
				}]
			}
		},
		methods: {
			async getdetail() {
				const res = await myRequest({
					url: "/api/getProductDetail/" + this.id,
				})
				// console.log(res.data.message[0])
				this.product = res.data.message[0]
				this.price = this.product.product_price;
				this.title = this.product.product_title;
				this.detail = this.product.product_detail;
				this.owner = this.product.product_owner;
				this.ownerName = this.product.user_name;
				this.status = this.product.product_status;
				this.discrict = this.product.product_district;
				// this.imgArr = this.product.imgArr;
				for (let i of this.product.imgArr) {
					this.imgArr.push(this.baseUrl + i)
				}
			},
			buy() {
				uni.navigateTo({
					url: "/pages/comfirmBuy/comfirmBuy?id=" + this.id
				})
			},
			formatDate(time) {
				var date = new Date(parseInt(time));
				var mon = date.getMonth() + 1;
				var day = date.getDate();
				return mon + '/' + day;
			},
			previewImg(current) {
				uni.previewImage({
					current,
					urls: this.imgArr
				})
			},
			async setHistory() {
				const res = await myRequest({
					url: "/api/setHistory",
					method: "POST",
					data: {
						product: this.id,
						user: this.user.user_id
					}
				})
			},
			edit() {
				uni.redirectTo({
					url: "../editItem/editItem?id=" + this.id
				})
			},
			changeStatus(status) {
				let _self = this;
				let content = status == 0 ? "是否终止交易" : "是否完成交易"
				uni.showModal({
					title: "提示",
					content,
					showCancel: true,
					confirmText: "确认",
					success(res) {
						if (res.confirm == true) {
							_self.changeProductStatus(status)
							uni.switchTab({
								url: "../shoppingList/shoppingList"
							})
						} else {}
					}
				})
			},
			async changeProductStatus(status) {
				let _self = this;
				const res = await myRequest({
					url: "/api/changeProductstatus",
					method: "POST",
					data: {
						status,
						productid: _self.id
					}
				})
			},
			async addFollow() {
				if (!this.isFollow) {
					let _self = this;
					const res = await myRequest({
						url: "/api/addFollow",
						method: "POST",
						data: {
							userid: _self.user.user_id,
							productid: _self.id
						}
					})
					if (res.data.status == 0) {
						uni.showToast({
							title: "关注成功"
						})
						_self.isFollow = true;
					}
				} else {
					let _self = this;
					const res = await myRequest({
						url: "/api/delFollow",
						method: "POST",
						data: {
							userid: _self.user.user_id,
							productid: _self.id
						}
					})
					if (res.data.status == 0) {
						uni.showToast({
							title: "取消关注成功",
							icon: "none"
						})
						_self.isFollow = false;
					}
				}
			},
			async findFollow() {
				let _self = this;
				const res = await myRequest({
					url: "/api/findFollow",
					method: "POST",
					data: {
						userid: _self.user.user_id,
						productid: _self.id
					}
				})
				if (res.data.message.length > 0) {
					_self.isFollow = true;
				}
			},

		},
		mounted() {
			this.getdetail();
			this.setHistory();
			this.baseUrl = this.$baseUrl;
		},
		onLoad(obj) {
			this.user = this.$getUser();
			this.id = obj.id;
			this.findFollow();
			this.isLogin = this.$checkLogin("", false);
		},
		onShow() {

		}
	}
</script>

<style scoped lang="scss">
	.itemDetails {
		image{
			width: 60rpx;
			height: 60rpx;
		}
		.swiper {
			width: 750rpx;
			height: 380rpx;
			text-align: center;

			image {
				height: 100%;
				width: 100%;
			}
		}

		.follow {
			display: flex;
			
			align-items: center;
			justify-content: center;
			image{
				width: 60rpx;
				height: 60rpx;
			}
		}

		// .icon-not-favorite:before {
		// 	content: '\e909';
		// 	transition: linear 0.5s;
		// }

		// .icon-favorite:before {
		// 	content: '\e907';
		// 	color: #ff0000;
		// 	transition: linear 0.5s;
		// }

		// .icon-ban:before {
		// 	content: '\e901';
		// }

		.owner {
			padding: 0 30rpx;
			margin: 10rpx 0;
			display: flex;
			height: 100rpx;
			position: relative;
			top: 0;
			align-items: center;
			background: #FFFFFF;

			.head {
				width: 15%;
			}

			.owner-name {
				width: 65%;
			}

			.discrict {
				width: 14%;
				border: 1px solid #000000;
				border-radius: 50rpx;
				padding: 0 10rpx;
			}
		}

		.steps {
			margin: 30rpx 0;
		}

		.connect{
			font-size: 60rpx;
		}
		
		.box {
			padding: 30rpx;
			position: relative;
			top: 0;

			.name {
				font-size: 60rpx;
			}

			.price-num {
				position: absolute;
				right: 5%;
				color: #C30602;
				line-height: 80rpx;
				font-size: 40rpx;
			}
			
			.tip{
				padding-top: 50rpx;
			}

			.detail {
				white-space: pre-wrap;
				padding-top: 20rpx;
			}

			.scroll-box {
				white-space: nowrap;
			}
		}

		.buttom {
			height: 200rpx;
			width: 1rpx;
		}

		.buttom-line {
			width: 100%;
			position: fixed;
			bottom: 0;
			display: flex;
			align-items: center;
			justify-content: center;
			background: #FFFFFF;
			height: 120rpx;


			button {
				width: 40%;
			}

			.follow{
				display: flex;
				align-items: center;
				justify-content: center;
				image{
					width: 40rpx;
					height: 40rpx;
				}
			}
			
			.buy{
				display: flex;
				align-items: center;
				justify-content: center;
				image{
					width: 40rpx;
					height: 40rpx;
				}
			}
			.edit {
				width: 30%;
			}

		}
	}
</style>
