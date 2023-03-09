<template>
    <view>
        <button type="primary" @click="wxLogin">微信登录</button>
 
    </view>
</template>
 
<script>
    export default {
        data() {
            return {
 
            };
        },
		methods: {
			wxLogin() {
				uni.login({
				    provider: 'weixin',
				    success: function (loginRes) {
				        // 登录成功
				        uni.getUserInfo({
				            provider: 'weixin',
				            success: function(info) {
				                // 获取用户信息成功, info.authResult保存用户信息
								console.log(info.authResult);
				            }
				        })
				    },
				    fail: function (err) {
				        // 登录授权失败  
				        // err.code是错误码
				    }
				});
			}
		}
    }
</script>
 
<style>
 
</style>