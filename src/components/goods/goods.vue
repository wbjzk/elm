<template>
	<div class="goods">
		<div class="menu-wrapper" ref="menuWrapper">
			<ul>
				<li class="menu-item" :class="{ 'current':currentIndex === i }" v-for="(item, i) in goods" :key="i" @click="selectMenu(i, $event)">
					<span class="text border-1px">
						<v-icon v-show="item.type > 0" :type="item.type" :num="3"></v-icon>{{ item.name }}
					</span>
				</li>
			</ul>
		</div>
		<div class="foods-wrapper" ref="foodsWrapper">
			<ul>
				<li class="food-list food-list-hook" v-for="(item, i) in goods" :key="i">
					<h2 class="title">{{ item.name }}</h2>
					<ul>
						<li class="food-item" v-for="(food, i) in item.foods" :key="i">
							<div class="icon">
								<img :src="food.icon" width="57" height="57">
							</div>
							<div class="content">
								<h3 class="name">{{ food.name }}</h3>
								<p class="desc">{{ food.description }}</p>
								<div class="extra">
									<span class="count">月售{{ food.sellCount }}</span>
									<span>好评率{{ food.rating }}</span>
								</div>
								<div class="price">
									<span class="now">￥{{ food.price }}</span>
									<span class="old" v-show="food.oldPrice">{{ food.oldPrice }}</span>
								</div>
								<div class="cartcontrol-wrapper">
									<!-- <div is="v-cartcontrol" :food="food"></div> -->
									<v-cartcontrol :food="food" @add="cartAdd"></v-cartcontrol>
								</div>
							</div>
						</li>
					</ul>
				</li>
			</ul>
		</div>
		<v-shopcart
			ref="shopcart"
			:delivery-price="seller.deliveryPrice"
			:min-price="seller.minPrice"
			:selectFoods="selectFoods">
		</v-shopcart>
	</div>
</template>
<script>
import BScroll from 'better-scroll';
import icon from '@/components/icon/icon.vue';
import shopcart from '@/components/shopcart/shopcart.vue';
import cartcontrol from '../cartcontrol/cartcontrol.vue';
const ERR_OK = 0;
export default {
	props: {
		seller: {
			required: false,
			type: Object,
		},
	},
	data() {
		return {
			goods: [],
			listHeight: [],
			scrollY: 0,
		};
	},
	created() {
		this.axios('api/goods').then(res => {
			if (res.data.errno === ERR_OK) {
				this.goods = res.data.goods;
				this.$nextTick(() => {
					this._initScroll();
					this._calculateHeight();
				});
			}
		});
	},
	methods: {
		selectMenu(index, event) {
			if(!event._constructed) { // 过滤原生事件
				return;
			}
			let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook'); // 获取右侧DOM元素列表
			let el = foodList[index]; // 获取DOM元素
			this.foodsScroll.scrollToElement(el, 500); // 调整滚动到对应位置
		},
		_initScroll() {
			this.menuScroll = new BScroll(this.$refs.menuWrapper, {
				click: true,	// 启用单击事件
			});
			this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
				probeType: 3,
				click: true,
			});
			this.foodsScroll.on('scroll', pos => {
				this.scrollY = Math.abs(Math.round(pos.y));
			});
		},
		_calculateHeight() {
			let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
			let height = 0;
			this.listHeight.push(height);
			for (let i = 0; i < foodList.length; i++) {
				let item = foodList[i];
				height += item.clientHeight;
				this.listHeight.push(height);
			}
		},
		cartAdd(target) {
			this.$nextTick(() => {
				this.$refs.shopcart.drop(target);
			});
		}
	},
	computed: {
		currentIndex() {
			for (let i = 0; i < this.listHeight.length; i++) {
				let height1 = this.listHeight[i];
				let height2 = this.listHeight[i+1];
				if(!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
					return i;
				}
			}
			return 0;
		},
		selectFoods() {
			let foods = [];
			this.goods.forEach(good => {
				good.foods.forEach(food => {
					if (food.count) {
						foods.push(food);
					}
				});
			});
			return foods;
		}
	},
	components: {
		'v-icon': icon,
		'v-shopcart': shopcart,
		'v-cartcontrol': cartcontrol,
	}
};
</script>
<style lang="less" scoped>
@import "../../common/style/mixin.less";
* {
	touch-action: pan-y;
}
.goods {
	display: flex;
	position: absolute;
	top: 174px;
	bottom: 46px;
	width: 100%;
	overflow: hidden;
	.menu-wrapper {
		flex: 0 0 80px;
		width: 80px;
		background: #f3f5f7;
		.menu-item {
			display: table;
			height: 54px;
			width: 56px;
			line-height: 14px;
			padding: 0 12px;
			line-height: 14px;
			&.current {
				position: relative;
				margin-top: -1px;
				z-index: 10;
				background-color: #fff;
				font-weight: bold;
				.text {
					.border-none();
				}
			}
			.icon {
				display: inline-block;
				vertical-align: top;
				width: 12px;
				height: 12px;
				margin-right: 2px;
				background-size: 12px 12px;
				background-repeat: no-repeat;
			}
			.text {
				display: table-cell;
				width: 56px;
				vertical-align: middle;
				.border-1px(rgba(7,17,27,.1));
				font-size: 12px;
			}
		}
	}
	.foods-wrapper {
		flex: 1;
		.food-list {
			.title {
				padding-left: 14px;
				height: 26px;
				line-height: 26px;
				border-left: 2px solid #d9dde1;
				font-size: 12px;
				color: rgb(147,153,159);
				background: #f4f5f7;
			}
			.food-item {
				display: flex;
				margin: 18px;
				padding-bottom: 18px;
				.border-1px(rgba(7,17,27,.1));
				&:last-child {
					.border-none();
					margin-bottom: 0;
				}
				.icon {
					flex: 0 0 57px;
					margin-right: 10px;
				}
				.content {
					flex: 1;
					.name {
						margin: 2px 0 8px 0;
						height: 14px;
						line-height: 14px;
						font-size: 14px;
						color: rgb(7,17,27);
					}
					.desc,
					.extra {
						line-height: 10px;
						font-size: 10px;
						color: rgb(147,153,159);
					}
					.desc {
						margin-bottom: 8px;
						line-height: 12px;
					}
					.extra {
						& .count {
							margin-right: 12px;
						}
					}
					.price {
						font-weight: bold;
						line-height: 24px;
						.now {
							margin-right: 8px;
							font-size: 14px;
							color: rgb(240, 20, 20);
						}
						.old {
							text-decoration: line-through;
							font-size: 10px;
							color: rgb(147, 153, 159);
						}
					}
					.cartcontrol-wrapper {
						position: absolute;
						right: 0;
						bottom: 12px;
					}
				}
			}
		}
	}
}
</style>
