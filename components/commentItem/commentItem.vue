<!-- <template>
	<view>
		<view class="postman">
			fabuzhe
		</view>
		
		<view class="detail">
			这是评论内容
			这是评论内容
			这是评论内容
		</view>
		
		<view class="time">
			这是时间
		</view>
	</view>
</template> -->


<template>
	<view class="container">
		<view class="info-text">
			<view class="name">zhangsan</view>
			<view class="content">这件商品真不错，性价比很高！</view>
			<view class="time">2022-12-16 15:00</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "commentItem",
		data() {
			return {
			
			}
		}
	}
</script>


<style lang="scss">
	.container {
		padding: 20px;
	}

	.comment-item {
		display: flex;
		align-items: center;
		margin-bottom: 20px;
	}

	.info-text {
		margin-left: 0;
	}

	.name {
		font-size: 30px;
		font-weight: bold;
	}

	.content {
		margin-top: 6px;
		font-size: 28px;
	}

	.time {
		margin-top: 6px;
		font-size: 24px;
		color: #999;
	}
</style>



