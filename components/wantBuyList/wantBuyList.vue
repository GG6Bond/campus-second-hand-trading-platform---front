<template>
	<view class="wantBuyItem">

		<view class="left_pic">
			<image src="../../static/indexpage/qiugou.png" mode="aspectFill"></image>
		</view>

		<view class="right_content">
			<view class="wantBuyName">
				这是名称+{{item.email}}
			</view>

			<view class="wantDetail">
				这是详情+{{item.email}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "wantBuyList",
		data() {
			return {

			};
		},
		props: {
			item: {
				wantBuyName: '',
				email: '',
				id: 1
			}
		},
		methods: {
			getList() {
				console.log('获取求购列表' + this.wantList);
			}
		},
	}
</script>

<style lang="scss">
	.wantBuyItem {
		height: 200rpx;
		display: flex;


		.left_pic {}

		.right_content {
			padding-left: 20rpx;

			.wantBuyName {
				font-size: 50rpx;
				width: 240px;
				overflow: hidden;
				text-overflow: ellipsis;
				white-space: nowrap;
			}

			.wantDetail {
				padding-top: 25rpx;
				width: 240px;
				overflow: hidden;
				text-overflow: ellipsis;
				white-space: nowrap;

			}
		}
	}





	image {
		height: 150rpx;
		width: 240rpx;
		padding-left: 30rpx;

		img {
			height: 100%;
			width: 100%;
		}
	}
</style>
