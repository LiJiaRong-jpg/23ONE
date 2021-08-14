<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
	* {
		margin: 0;
		padding: 0;
	}

	@font-face {
	  font-family: 'iconfont';  /* Project id 2537631 */
	  src: url('https://at.alicdn.com/t/font_2537631_dx47wskjxnj.woff2?t=1628821739001') format('woff2'),
	       url('https://at.alicdn.com/t/font_2537631_dx47wskjxnj.woff?t=1628821739001') format('woff'),
	       url('https://at.alicdn.com/t/font_2537631_dx47wskjxnj.ttf?t=1628821739001') format('truetype');
	}

	.iconfont {
		width: 7vh;
		font-family: "iconfont" !important;
		font-size: 25px;
		color: black;
		font-style: normal;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
	}
</style>
