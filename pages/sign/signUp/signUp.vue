<template>
	<view class="sign-up">
		手机号：
		<input v-model="id" type="number" placeholder="请输入手机号" />
		用户名：
		<input v-model="username" type="text" placeholder="请输入用户名" />
		密码：
		<input v-model="password" type="password" placeholder="请输入密码" />
		再次输入密码：
		<input v-model="chkpass" type="password" placeholder="确认密码" />
		<button @click="check" class="btn">注册</button>
	</view>
</template>

<script>
	import {
		myRequest
	} from "@/util/api.js"
	export default {
		data() {
			return {
				id: "",
				username: "",
				password: "",
				chkpass: ""
			}
		},
		methods: {
			check() {
				if (!this.id) {
					uni.showToast({
						title: "请输入手机号",
						icon: "error"
					})
				} else if (!this.password) {
					uni.showToast({
						title: "请输入密码",
						icon: "error"
					})
				} else if (this.password !== this.chkpass) {
					uni.showToast({
						title: "密码不一致！",
						icon: "error"
					})
				} else if (!this.username) {
					uni.showToast({
						title: "请输入用户名",
						icon: "error"
					})
				} else {
					this.createUser();
				}
			},
			async createUser() {
				const res = await myRequest({
					url: "/api/createUser/",
					method: "POST",
					data: {
						id: this.id,
						password: this.password,
						username: this.username,
					}
				})
				if (res.data.status !== 0) {
					return uni.showToast({
						title: "用户已存在！",
						icon: "error"
					})
				} else {
					this.id = "";
					this.username = "";
					this.password = "";
					this.chkpass = "";
					uni.showToast({
						title: "注册成功！即将跳转至登录页",
						icon: "none"
					})
					setTimeout(() => {
						uni.redirectTo({
							url: "../signIn/signIn"
						})
					}, 3000)
				}
			}
		}
	}
</script>


<style lang="scss">
	.sign-up {
		margin: 40rpx;
		line-height: 80rpx;
		font-size: 50rpx;

		input {
			border: 1px solid #3F536E;
			border-radius: 20rpx;
			height: 90rpx;
			padding: 10rpx;
			margin-bottom: 20rpx;
		}
		
		.btn {
			margin-top: 50rpx;
		}
	}
</style>
