<!-- 求购商品详情页面 -->
<template>
	<view class="content">
		<view class="name">
			<u-tag text="名称" type="info" />
			<view class="">
				{{name}}
			</view>
		</view>


		
		<view class="detail">
			<u-tag text="详情" type="info" />
			<u-read-more :toggle="true" show-height="100">
				<rich-text :nodes="deatil"></rich-text>
			</u-read-more>
		</view>


		<view class="email">
			<u-tag text="联系方式" type="info" />
			<view class="">
				{{email}}
			</view>
		</view>


		<view class="postid">
			<u-tag text="发布人" type="info" />
			<view class="">
				{{postid}}
			</view>
		</view>
	</view>
</template>

<script>
	import {
		myRequest
	} from '../../util/api';
	export default {
		data() {
			return {
				wantBuyDeatil: {},
				name: '',
				deatil: '',
				email: '',
				postid: '',
			};
		},
		onLoad(e) {
			console.log(e.id);
			this.getDetail(e.id);


		},
		methods: {
			async getDetail(e) {
				const res = await myRequest({
					url: "/api/getWantBuyDetail/" + e,
					method: 'POST'
				})
				this.wantBuyDeatil = res.data;

				console.log(this.wantBuyDeatil.message[0].id);
				console.log(this.wantBuyDeatil.message[0].wantBuyName);
				console.log(this.wantBuyDeatil.message[0].wantBuyDetail);
				console.log(this.wantBuyDeatil.message[0].user_id);
				console.log(this.wantBuyDeatil.message[0].email);

				this.name = this.wantBuyDeatil.message[0].wantBuyName;
				this.deatil = this.wantBuyDeatil.message[0].wantBuyDetail;
				this.email = this.wantBuyDeatil.message[0].email;
				this.postid = this.wantBuyDeatil.message[0].user_id;


			}
		}
	}
</script>

<style lang="scss">
	.content {

		// font-size: 50rpx;

		.name {
			text-align: center;
			font-size: 60rpx;
		}

		u-gap {}

		rich-text {
			font-size: 40rpx;
		}


		.email {
			padding-bottom: 30rpx;
			font-size: 40rpx;
		}

		.postid {
			padding-bottom: 30rpx;
			font-size: 40rpx;
		}
	}
</style>
