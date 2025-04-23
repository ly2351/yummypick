<template>
	<view class="container">
		<!-- 顶部欢迎区域 -->
		<uni-section class="welcome-section" type="line">
			<view class="welcome-content">
				<text class="welcome-text">Hey, 上午好！</text>
				<uni-tag text="VIP1" type="success" size="small" class="vip-tag" />
			</view>
		</uni-section>

		<!-- 会员信息卡片 -->
		<view class="custom-card" @click="onCardClick">
			<!-- 卡片头部 -->
			<view v-if="title || subTitle" class="custom-card__header">
				<view class="custom-card__header-box" @click="onCardClick('header')">
					<!-- 标题和副标题 -->
					<view class="custom-card__header-content">
						<text class="custom-card__header-content-title uni-ellipsis">{{ title }}</text>
						<text v-if="subTitle"
							class="custom-card__header-content-subtitle uni-ellipsis">{{ subTitle }}</text>
					</view>
				</view>
				<!-- 额外信息 -->
				<view v-if="extra" class="custom-card__header-extra" @click="onCardClick('extra')">
					<text class="custom-card__header-extra-text">{{ extra }}</text>
				</view>
			</view>
			<view class="custom-card-line">

			</view>
			<!-- 卡片内容区域 -->
			<view class="custom-card__content" @click="onCardClick('content')">
				<uni-grid :column="4" :showBorder="false">
					<uni-grid-item>
						<text class="card-value">179</text>
						<text class="card-label">储值</text>

					</uni-grid-item>
					<uni-grid-item>
						<text class="card-value">0</text>
						<text class="card-label">积分</text>

					</uni-grid-item>
					<uni-grid-item>
						<text class="card-value">2张</text>
						<text class="card-label">权益卡</text>

					</uni-grid-item>
					<uni-grid-item @click="handleScanCode">
						<uni-icons type="scan" size="25" color="#000" class="card-value" />
						<text class="card-label">会员码</text>
					</uni-grid-item>
				</uni-grid>
			</view>

		</view>

		<!-- 功能选项列表 -->
		<uni-list>
			<uni-list-item v-for="(item, index) in listItems" :key="index" :title="item.title" :thumb="item.icon"
				@click="navigateTo(item.path)" />
		</uni-list>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				listItems: [{
						title: '我的优惠券',
						icon: '/static/coupon.png',
						path: '/pages/coupons/coupons'
					},
					{
						title: '我的订单',
						icon: '/static/order.png',
						path: '/pages/orders/orders'
					},
					{
						title: '收货地址',
						icon: '/static/address.png',
						path: '/pages/address/address'
					},
					{
						title: '个人资料',
						icon: '/static/profile.png',
						path: '/pages/profile/profile'
					},
					{
						title: '我的会员卡',
						icon: '/static/card.png',
						path: '/pages/card/card'
					},
					{
						title: '关于',
						icon: '/static/about.png',
						path: '/pages/about/about'
					}
				],
				title: 'VIP1',
				subTitle: '欢迎加入本店会员~',
				extra: '会员权益',
				thumbnail: 'https://example.com/avatar.jpg', // 替换为你的头像 URL
			}
		},
		methods: {
			navigateTo(path) {
				uni.navigateTo({
					url: path
				})
			},
			handleScanCode() {
				uni.scanCode({
					success: (res) => {
						uni.showToast({
							title: '扫描结果：' + res.result
						})
					}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	/* 全局样式 */
	:root {
		--primary-color: #FFA500;
		--text-primary: #333;
		--text-secondary: #666;
		--border-color: #eee;
	}

	.container {
		padding-bottom: 120rpx;
		/* 底部导航栏高度 */
	}

	/* 会员信息卡片 */
	.custom-card {
		background-color: #fff;
		// margin-bottom: 20px;
		margin: 20rpx;
		border-radius: 16rpx;
		padding: 30rpx 30rpx 0 30rpx;
	}

	.custom-card__header {
		display: flex;
		justify-content: space-between;
		/* 使左侧和右侧元素分开 */
		align-items: center;
		/* 垂直居中对齐 */

	}

	.custom-card-line {
		margin: 25rpx 0 35rpx 0;
		border-bottom: #eee 1rpx solid;
	}

	.custom-card__header-box {
		flex-grow: 1;
		/* 让标题和副标题区域占据剩余空间 */
	}

	.custom-card__header-content {
		display: flex;
		flex-direction: column;
	}

	.custom-card__header-content-title {
		font-weight: bold;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}

	.custom-card__header-content-subtitle {
		color: #888;
		font-size: 0.675em;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}

	.custom-card__header-extra {
		text-align: right;
	}

	.custom-card__header-extra-text {
		color: black;
		font-size: 0.675em;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
		border: 1rpx darkgray solid;
		border-radius: 30rpx;
		padding: 15rpx;
	}

	.custom-card__content {
		.card-value {
			color: black;
			font-size: larger;
			font-weight: bolder;
			white-space: nowrap;
			overflow: hidden;
			text-overflow: ellipsis;
			margin-bottom: 10rpx;
			text-align: center;
		}

		.card-label {
			color: #888;
			font-size: 0.675em;
			white-space: nowrap;
			overflow: hidden;
			text-overflow: ellipsis;
			text-align: center;
		}
	}

	/* 顶部欢迎区域 */
	.welcome-section {
		background-color: var(--primary-color);
		padding: 40rpx 0;

		.welcome-content {
			display: flex;
			justify-content: center;
			align-items: center;
			gap: 20rpx;
		}

		.welcome-text {
			font-size: 36rpx;
			font-weight: bold;
			color: white;
		}

		.vip-tag {
			padding: 4rpx 16rpx;
			border-radius: 16rpx;
			background-color: rgba(255, 255, 255, 0.9);
		}
	}
</style>