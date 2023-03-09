<template>
	<view class="editUser">
		<rowTextEnter :data="rowTextEnter"></rowTextEnter>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				user: [],
				rowTextEnter: [{
						text: "昵称",
						righttext: "",
						url: "../changeUser/changeUser?index="
					},
					{
						text: "手机号",
						righttext: "",
						url: "../changeUser/changeUser?index="
					},
					{
						text: "密码",
						righttext: "",
						url: "../changeUser/changeUser?index="
					},
					// {
					// 	text: "用户权限",
					// 	righttext: ""
					// },
				]
			};
		},

		onLoad(obj) {
			this.user = this.$getUser();
			this.rowTextEnter[0].righttext = this.user.user_name;
			this.rowTextEnter[1].righttext = this.user.user_id;
		}
	}
</script>

<style lang="scss">
	.editUser {}
</style>
