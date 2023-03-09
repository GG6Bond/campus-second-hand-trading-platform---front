<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			// console.log('App Show')
		},
		onHide: function() {
			// console.log('App Hide')
		},
		onLoad() {

		}
	}
</script>

<style>
	/*每个页面公共css */

	/* 字体图标 */
	@import url("@/static/font/style.css");

	page {
		background: #f1f1f1;
	}

	/* #ifdef H5 */
	uni-radio .uni-radio-input-checked {
		background: #1b92ff !important;
	}

	/* #endif */

	/* #ifdef MP-WEIXIN */
	wx-radio .wx-radio-input-checked {
		background: #1b92ff !important;
		border-color: #1b92ff !important;
	}

	/* #endif */
</style>
