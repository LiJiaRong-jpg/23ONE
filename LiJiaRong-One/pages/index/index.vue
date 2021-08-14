<template>
	<view id="content">
		<view class="Input-Nav">
			<view class="input">
				<view>
					<text class="iconfont">&#xe621;</text>
				</view>
				<view>
					<text class="iconfont">&#xe632;</text>
					<input type="text" placeholder="智能积木 越野四驱车" />
				</view>
				<view>
					<text class="iconfont">&#xe60e;</text>
				</view>
			</view>
			<view class="nav">
				<scroll-view class="scroll-view_H" show-scrollbar="true" scroll-x="true">
					<text :class="[nav == item.id ? 'navColor' : '']" v-for="item in reds.category"
						:key='item.id'>{{item.name}}</text>
				</scroll-view>
			</view>
		</view>
		<view class="content">
			<swiper class="swiper" circular="true" indicator-dots="true" autoplay="true" interval="3000" duration="500">
				<!-- 轮播图 -->
				<swiper-item v-for="item in reds.data[0].data" :key="item.id">
					<image :src="item.src"></image>
				</swiper-item>
			</swiper>
			<view class="RotationRow">
				<!-- 轮播图下的小图标 -->
				<view v-for="item in reds.data[1].data" :key="item.id">
					<image :src="item.src" mode="widthFix"></image>
					<text>{{item.text}}</text>
				</view>
			</view>
			<view style="width: 100%;display: flex;margin-bottom: 5px;">
				<view style="width:50%;float: left;">
					<image :src="reds.data[2].data[0].src" mode="widthFix" style="width:100%"></image>
				</view>
				<view style="width:50%;float: right;">
					<image :src="reds.data[2].data[1].src" mode="widthFix" style="width:100%"></image>
					<image :src="reds.data[2].data[2].src" mode="widthFix" style="width:100%"></image>
				</view>
			</view>
			<view class="ProductList">
				<!-- 商品列表 -->
				<view v-for="item in reds.data[4].data" :key="item.id">
					<image :src="item.cover" mode="widthFix"></image>
					<text> {{item.title}}\n</text>
					<text style="color: #C0C0C0;"> {{item.desc}}\n</text>
					<text style="color: red;"> ￥{{item.pprice}}</text>
					<text style="text-decoration: line-through;color: #C0C0C0;"> ￥{{item.oprice}}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				reds: { //请求的数据
					category: [], //导航栏
					data: [] //0是轮播图，1是轮播图下的小图标，2是商品列表上的3个图片，3是不知道，4是商品列表
				},
				nav: "1",
				page: '2', //加载的页数
			}
		},
		mounted() {
			let height = document.getElementsByClassName('Input-Nav')[0]
			let tar = document.getElementsByClassName('content')[0]
			tar.style.paddingTop = height.offsetHeight + 'px'
		},
		onLoad() {
			this.Load()
		},
		methods: {
			Load() { //首次加载时调用
				let that = this
				uni.request({ //首页数据
					url: 'http://ceshi3.dishait.cn/api/index_category/data',
					success: (res) => {
						console.log(res)
						that.reds = res.data.data
					}
				})
			},
			onReachBottom() { //页面滚动到底部
				this.LoadNextPage()
			},
			LoadNextPage() { //加载下一页
				let that = this
				uni.showLoading({
					title: '加载中'
				});
				uni.request({ //首页数据
					url: 'http://ceshi3.dishait.cn/api/index_category/1/data/' + that.page,
					success: (res) => {
						if (0 < Object.getOwnPropertyNames(res.data.data[2].data).length) {//如果有新数据
						for (let key in res.data.data[2].data) {
							that.reds.data[4].data[Object.getOwnPropertyNames(that.reds.data[4].data).length -
								1] = res.data.data[2].data[key]//得到商品列表的长度，以此为下标，将对应得新数据加入到商品列表中
						}
							that.$forceUpdate()//刷新视图
						}
						uni.hideLoading();
					},
					fail() {
						uni.hideLoading();
					}
				})
			}
		}
	}
</script>

<style>
	.Input-Nav {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		z-index: 999;
		background-color: white;
	}

	.input {
		margin-top: 5px;
		display: flex;
	}

	.nav {
		margin: 5px 0;
	}

	.input>view:nth-child(1),
	.input>view:nth-child(3) {
		width: 10%;
		float: left;
		text-align: center;
	}

	.input>view:nth-child(2)>.iconfont {
		float: left;
		color: darkgray;
	}

	.input input {
		width: 70%;
		float: left;
	}

	.input>view:nth-child(2) {
		background-color: rgb(247, 247, 247);
		overflow: hidden;
		float: left;
		width: 80%;
		padding-left: 10px;
		box-sizing: border-box;
	}

	.scroll-view_H {
		width: 100%;
		white-space: nowrap;
	}

	.nav text {
		margin: 0 10px;
	}

	.navColor {
		color: red;
	}

	.swiper image {
		width: 100%;
	}

	.RotationRow {
		width: 100%;
		margin: 5px 0;
	}

	.RotationRow>view {
		width: 20%;
		text-align: center;
		float: left;
	}

	.RotationRow>view>image {
		width: 50%;
		margin: 0 25%;
	}

	.ProductList {
		width: 100%;
	}

	.ProductList>view {
		width: 48%;
		margin: 0 1%;
		float: left;
	}

	.ProductList>view>image {
		width: 100%;
	}
</style>
