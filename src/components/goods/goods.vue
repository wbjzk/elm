<template>
	<div class="goods">
		<div class="menu-wrapper" ref="menuWrapper">
			<ul>
				<li class="menu-item" v-for="(item, i) in goods" :key="i">
					<span class="text border-1px">
						<v-icon v-show="item.type > 0" :type="item.type" :num="3"></v-icon>{{ item.name }}
					</span>
				</li>
			</ul>
		</div>
		<div class="foods-wrapper" ref="foodsWrapper">
			<ul>
				<li class="food-list" v-for="(item, i) in goods" :key="i">
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
							</div>
						</li>
					</ul>
				</li>
			</ul>
		</div>
	</div>
</template>
<script>
import BScroll from 'better-scroll';
import icon from '@/components/icon/icon.vue';
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
			goods: []
		};
	},
	created() {
		this.axios('api/goods').then(res => {
			if (res.data.errno === ERR_OK) {
				this.goods = res.data.goods;
				this.$nextTick(() => {
					this._initScroll();
				});
			}
		});
	},
	methods: {
		_initScroll() {
			this.menuScroll = new BScroll(this.$refs.menuWrapper, {});
			this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {});
		}
	},
	components: {
		'v-icon': icon
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
				}
			}
		}
	}
}
</style>