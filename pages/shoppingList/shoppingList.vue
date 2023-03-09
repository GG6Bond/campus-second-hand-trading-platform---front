<template>
	<view>
		<view class="search">
			<searchBar @searchResult="getResult"></searchBar>
			<!-- <search-bar @searchResult="getResult"></search-bar> -->
		</view>
		<shoppingListItem :data="shoppingList"></shoppingListItem>
		<view class="uni-pagination-box">
			<uni-pagination show-icon :page-size="pageSize" :current="pageCurrent" :total="total" @change="change" />
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
				shoppingList: [],
				shoppingListItem: [],
				user: {},
				isLogin: false,
				// 每页数据量
				pageSize: 5,
				// 当前页
				pageCurrent: 1,
				total: 0
			};
		},

		methods: {
			// 分页触发
			change(e) {
				this.pageCurrent = e.current
				this.changeList();
			},
			changeList() {
				let start = (this.pageCurrent - 1) * this.pageSize;
				let end = this.pageCurrent * this.pageSize;
				if (end > this.shoppingListItem.length) {
					end = this.shoppingListItem.length;
				}
				// console.log("start", start)
				// console.log("end", end)
				// console.log(this.data.slice(start, end))
				this.shoppingList = this.shoppingListItem.slice(start, end);
				this.total = this.shoppingListItem.length;
			},
			getResult(data) {
				this.shoppingListItem = data;
				this.changeList();
			},
			async getProductList() {
				const res = await myRequest({
					url: "/api/getProductList/" + this.user.user_id
				})
				this.shoppingListItem = res.data.message;
				this.total = res.data.message.length;
				this.changeList();
				// console.log(this.shoppingListItem)
			}
		},
		watch: {
			isLogin(newVal, oldVal) {
				this.getProductList();
			}
		},
		onPullDownRefresh() {
			setTimeout(() => {
				this.getProductList();
				uni.stopPullDownRefresh();
			}, 1000)
		},
		onLoad() {
			this.getProductList();
			this.changeList()
		},
		onShow() {
			this.isLogin = this.$checkLogin("", false);
			this.user = this.$getUser();
			this.changeList()
		}
	}
</script>

<style lang="scss" scoped>
</style>
