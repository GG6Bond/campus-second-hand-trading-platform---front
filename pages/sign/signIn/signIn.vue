<!-- 登录 -->
<template>
	<view class="sign-in">
		用户名：
		<input v-model="username" type="number" placeholder="请输入手机号" />
		密码：
		<input v-model="password" type="password" placeholder="请输入密码" @keypress.native.enter="check" />
		<button class="btn" @click="check">登录</button>
		<view class="bottom-box">还没有账号？点击注册→ <span type="default" @click="gotoPage" class="register">注册</span></view>

		<!-- #ifdef MP-WEIXIN -->
		<!-- <wx-login></wx-login> -->
		<!-- #endif -->
	</view>
</template>

<script>
	import {
		myRequest
	} from "@/util/api.js"
	// #ifdef MP-WEIXIN
	import wxLogin from "@/pages/components/wxLogin/wxLogin.vue"
	// #endif
	export default {
		data() {
			return {
				username: "",
				password: ""
			};
		},
		// #ifdef MP-WEIXIN
		components: {
			wxLogin
		},
		// #endif
		methods: {
			check() {
				if (!this.username) {
					uni.showToast({
						title: "请输入手机号",
						icon: "error"
					})
				} else if (!this.password) {
					uni.showToast({
						title: "请输入密码",
						icon: "error"
					})
				} else {
					this.getPassword();
				}
			},
			async getPassword() {
				const res = await myRequest({
					url: "/api/getuserinfo/" + this.username
				})
				if (res.data.message.length === 0) {
					uni.showToast({
						title: "用户不存在",
						icon: "error",
					})
					return;
				}
				if (this.password === res.data.message[0].user_password) {
					this.setUser(res.data.message[0]);
					uni.showToast({
						title: "登录成功！",
						icon: "none",
					})
					uni.switchTab({
						url: "../../index/index"
					})

				} else {
					uni.showToast({
						title: "密码错误",
						icon: "error"
					})
				}
			},
			setUser(user) {
				uni.setStorageSync('user', user);
				uni.setStorageSync('isLogin', true);
			},
			gotoPage() {
				uni.redirectTo({
					url: "../signUp/signUp"
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.sign-in {
		margin: 40rpx;
		line-height: 80rpx;
		font-size: 50rpx;

		input {
			border: 1px solid #3F536E;
			border-radius: 20rpx;
			height: 90rpx;
			padding: 10rpx;
		}


		.btn {
			margin-top: 50rpx;
		}

		.bottom-box {
			height: 300rpx;
			line-height: 300rpx;
			font-size: 50rpx;
			position: fixed;
			bottom: 0;

			.register {
				color: #007AFF;
			}
		}
	}
</style>
