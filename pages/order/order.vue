<template>
  <view>
    <!-- Order list start -->
    <view class="order-list" v-for="(order, orderIndex) in orderList" :key="orderIndex">
      
      <!-- Order header -->
      <view class="order-header">
        <view class="order-info">
          <view class="order-status">{{ order.eatStatus }}</view>
          <view class="order-address">{{ order.address }}</view>
        </view>
        <view class="order-payment-status">{{ order.status }}</view>
      </view>
      
      <!-- Menu items -->
      <view v-for="(menuItem, menuIndex) in order.menu" :key="menuIndex" class="menu-item" @click="viewOrderDetail">
        <view>
          <image class="menu-item-image" :src="menuItem.icon" mode="aspectFill"></image>
        </view>
        <view class="menu-item-details">
          <text class="menu-item-name">{{ menuItem.name }}</text>
          <view class="menu-item-description">{{ menuItem.desc }}</view>
          <view class="menu-item-price">
            <view class="price-symbol">￥</view>
            <text class="price-amount">{{ menuItem.price }}</text>
            <view class="quantity">x{{ menuItem.num }}</view>
          </view>
        </view>
      </view>
      
      <!-- Order summary -->
      <view class="order-summary">
        共计{{ order.num }}件商品，合计：￥{{ order.price }}
      </view>
      
      <!-- Reorder button -->
      <view class="reorder-button" @click="placeNewOrder">
		<uni-tag text="再来一单" :circle="true" type="error" />
      </view>
    </view>
    <!-- Order list end -->
  </view>
</template>

<script>
export default {
  data() {
    return {
      orderList: [
        {
          eatStatus: '堂食',
          address: '私房菜（万达广场店）',
          status: '已完成',
          menu: [
            {
              icon: '/static/menu/menulist/gbyd.png',
              name: '干煸芸豆',
              desc: '芸豆+麻椒+老干妈+芝麻+辣椒',
              price: 16,
              num: 1
            }
          ],
          num: 1,
          price: 16
        },
        {
          eatStatus: '外卖',
          address: '北京市朝阳区万豪公馆...',
          status: '已取消',
          menu: [
            {
              icon: '/static/menu/menulist/tclj.png',
              name: '糖醋里脊',
              desc: '猪肉+醋+糖',
              price: 28,
              num: 1
            },
            {
              icon: '/static/menu/menulist/xhscjd.png',
              name: '西红柿炒鸡蛋',
              desc: '西红柿+高筋面粉+鸡蛋+淀粉',
              price: 14,
              num: 1
            },
            {
              icon: '/static/menu/menulist/mf.png',
              name: '米饭',
              desc: '五常大米+水',
              price: 2.5,
              num: 2
            }
          ],
          num: 4,
          price: 47
        }
      ]
    };
  },
  methods: {
    viewOrderDetail() {
      uni.navigateTo({
        url: "/subPack/order/orderDetail"
      });
    },
    placeNewOrder() {
      uni.switchTab({
        url: '/pages/menu/menu'
      });
    }
  }
};
</script>

<style lang="scss">
/* General container styles */
.order-list {
  margin: 30rpx;
  padding: 20rpx;
  box-shadow: 2px 0px 8px rgba(243, 244, 246, 0.95);
  background-color: #fff;
  border-radius: 10rpx;
}

/* Order header styles */
.order-header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20rpx;
}

.order-info {
  display: flex;
  flex-direction: column;
}

.order-status {
  font-weight: bold;
  font-size: 28rpx;
}

.order-address {
  font-size: 24rpx;
  color: #666;
}

.order-payment-status {
  font-size: 24rpx;
  color: #EE2F37;
  font-weight: bold;
}

/* Menu item styles */
.menu-item {
  display: flex;
  margin-top: 30rpx;
  cursor: pointer;
}

.menu-item-image {
  width: 100rpx;
  height: 100rpx;
  border-radius: 20rpx;
}

.menu-item-details {
  flex-grow: 1;
  margin-left: 20rpx;
}

.menu-item-name {
  font-size: 26rpx;
  font-weight: bold;
}

.menu-item-description {
  margin: 10rpx 0;
  font-size: 22rpx;
  color: #999;
}

.menu-item-price {
  display: flex;
  justify-content: space-between;
  font-size: 24rpx;
}

.price-symbol {
  color: #EE2F37;
  font-weight: bold;
}

.price-amount {
  color: #EE2F37;
  font-weight: bold;
}

.quantity {
  color: #999;
  font-size: 20rpx;
}

/* Order summary styles */
.order-summary {
  text-align: right;
  font-size: 22rpx;
  font-weight: bold;
  margin-top: 30rpx;
}

/* Reorder button styles */
.reorder-button {
  display: flex;
  justify-content: flex-end;
  margin-top: 30rpx;
}
</style>
