

<template>
	<view class="container">
		<view class="info-text">
			<view class="name">{{item.user_id}}</view>
			<view class="content">{{item.content}}</view>
			<view class="time">{{item.post_time}}</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "commentItem",
		data() {
			return {
			
			}
		},
		props:{
			item:{
				user_id:'',
				content:'',
				post_time:''
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



