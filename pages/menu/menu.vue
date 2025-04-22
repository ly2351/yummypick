<template>
  <view class="container">
    <!-- 左侧 Sidebar -->
    <view class="sidebar">
      <view 
        v-for="(item, index) in menuList" 
        :key="index" 
        :class="['sidebar-item', activeIndex === index ? 'active' : '']"
        @click="handleSidebarClick(index)"
      >
        <image :src="item.image" class="menu-image"/>
        <view>{{ item.name }}</view>
      </view>
    </view>

    <!-- 右侧内容区域 -->
    <scroll-view 
      class="content" 
      scroll-y 
      :scroll-into-view="scrollToView"
      @scroll="handleScroll"
    >
      <view 
        v-for="(item, index) in menuList" 
        :id="'section-' + index" 
        :key="index" 
        class="section"
      >
        <view class="section-title">{{ item.name }}</view>
        <view class="section-content">
          <!-- 菜品列表 -->
          <view v-for="(dish, dishIndex) in item.foods" :key="dishIndex" class="dish-item">
            <image :src="dish.icon" class="dish-icon"/>
            <view class="dish-info">
              <view class="dish-name">{{ dish.name }}</view>
              <view class="dish-desc">{{ dish.desc }}</view>
              <view class="dish-meta">
                <view class="price">¥{{ dish.price }}</view>
                <view class="sold">月售{{ dish.value }}份</view>
              </view>
            </view>
          </view>
        </view>
      </view>
    </scroll-view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      activeIndex: 0,
      scrollToView: '',
      menuList: [
        {
          name: '川味麻辣风',
          image:'/static/menu/menutab/cwmlf.png',
          foods: [
            {
              id: 1,
              icon: '/static/menu/menulist/gbyd.png',
              name: '干煸芸豆',
              desc: '芸豆+麻椒+老干妈+芝麻+辣椒',
              price: '16',
              value: 0
            },
            {
              id: 2,
              icon: '/static/menu/menulist/mpdf.png',
              name: '麻婆豆腐',
              desc: '豆腐+大葱+豆瓣酱+麻椒+干辣椒+葱',
              price: '12',
              value: 0
            },
            {
              id: 3,
              icon: '/static/menu/menulist/yxrs.png',
              name: '鱼香肉丝',
              desc: '猪肉+土豆丝+胡萝卜+青椒丝+大葱+木耳',
              price: '20',
              value: 0
            }
          ]
        },
        {
          name: '家常小炒',
          image:'/static/menu/menutab/jcxc.png',
          foods: [
            {
              id: 4,
              icon: '/static/menu/menulist/tclj.png',
              name: '糖醋里脊',
              desc: '猪肉+醋+糖',
              price: '28',
              value: 0
            },
            {
              id: 5,
              icon: '/static/menu/menulist/jjrs.png',
              name: '京酱肉丝',
              desc: '猪肉+香菜+豆腐皮+葱+糖+甜面酱',
              price: '26',
              value: 0
            },
            {
              id: 6,
              icon: '/static/menu/menulist/hgr.png',
              name: '回锅肉',
              desc: '猪五花肉+尖椒+蒜薹',
              price: '27',
              value: 0
            }
          ]
        },
        ,
        	{
        		name: '时蔬素菜',
        		image:'/static/menu/menutab/sssc.png',
        		foods: [{
        				id: 7,
        				icon: '/static/menu/menulist/sltds.png',
        				name: '酸辣土豆丝',
        				desc: '土豆+青椒+辣椒',
        				price: '11',
        				value: 0
        			},
        			{
        				id: 8,
        				icon: '/static/menu/menulist/xhscjd.png',
        				name: '西红柿炒鸡蛋',
        				desc: '西红柿+高筋面粉+鸡蛋+淀粉',
        				price: '14',
        				value: 0
        			},
        			{
        				id: 9,
        				icon: '/static/menu/menulist/hsqz.png',
        				name: '红烧茄子',
        				desc: '茄子+鸡蛋+辣椒+盐+青红椒',
        				price: '19',
        				value: 0
        			}
        		]
        	},
        	{
        		name: '汤品 主食',
        		image:'/static/menu/menutab/tpzs.png',
        		foods: [{
        				id: 10,
        				icon: '/static/menu/menulist/mf.png',
        				name: '米饭',
        				desc: '五常大米+水',
        				price: '2.5',
        				value: 0
        			},
        			{
        				id: 11,
        				icon: '/static/menu/menulist/mt.png',
        				name: '馒头',
        				desc: '高筋面粉+水',
        				price: '1.2',
        				value: 0
        			},
        			{
        				id: 12,
        				icon: '/static/menu/menulist/slt.png',
        				name: '酸辣汤',
        				desc: '猪肝+鸡蛋+豆腐+海参+芥菜+木耳+胡椒+醋',
        				price: '12',
        				value: 0
        			},
        			{
        				id: 13,
        				icon: '/static/menu/menulist/gdt.png',
        				name: '疙瘩汤',
        				desc: '面粉+鸡蛋+海米',
        				price: '16',
        				value: 0
        			}
        		]
        	}
      ]
    };
  },
  methods: {
    handleSidebarClick(index) {
      this.activeIndex = index;
      this.scrollToView = `section-${index}`;
    },
    handleScroll(e) {
      const scrollTop = e.detail.scrollTop;
      const sections = this.menuList.map((_, index) => {
        const query = uni.createSelectorQuery().in(this);
        query.select(`#section-${index}`).boundingClientRect();
        return new Promise(resolve => {
          query.exec(res => resolve(res[0].top));
        });
      });

      Promise.all(sections).then(topValues => {
        const currentIndex = topValues.findIndex(top => top >= 0 && top <= 50);
        if (currentIndex !== -1) {
          this.activeIndex = currentIndex;
        }
      });
    }
  }
};
</script>

<style scoped>
.container {
  display: flex;
  height: 100vh;
  font-family: 'Arial', sans-serif;
}

.sidebar {
  width: 25%;
  background-color: #ffffff;
  border-right: 1px solid #ebebeb;
  overflow-y: auto;
  box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1);
}

.sidebar-item {
  padding: 20px;
  text-align: center;
  color: #666;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
}

.sidebar-item:hover {
  color: #007aff;
}

.sidebar-item.active {
  color: #007aff;
  font-weight: bold;
}

.menu-image {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  margin-bottom: 8px;
}

.content {
  width: 75%;
  height: 100%;
  padding: 20px;
  overflow-y: auto;
  background-color: #ffffff;
}

.section {
  margin-bottom: 20px;
}

.section-title {
  font-size: 22px;
  font-weight: bold;
  color: #333;
  margin-bottom: 10px;
  padding-bottom: 10px;
  border-bottom: 2px solid #f0f0f0;
}

.dish-item {
  display: flex;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  margin-bottom: 15px;
  padding: 15px;
  align-items: center;
  transition: transform 0.3s ease;
}

.dish-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.15);
}

.dish-icon {
  width: 60px;
  height: 60px;
  border-radius: 8px;
  margin-right: 20px;
  object-fit: cover;
}

.dish-info {
  flex-grow: 1;
}

.dish-name {
  font-size: 18px;
  font-weight: bold;
  color: #333;
}

.dish-desc {
  font-size: 14px;
  color: #666;
  margin-top: 5px;
}

.dish-meta {
  display: flex;
  justify-content: space-between;
  font-size: 14px;
  color: #666;
  margin-top: 10px;
}

.price {
  font-weight: bold;
  color: #f00;
}

.sold {
  color: #999;
}
</style>
