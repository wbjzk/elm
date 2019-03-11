<template>
	<div class="seller" ref="seller">

		<div class="seller-content">
			<div class="overview">
				<h2 class="title">{{ seller.name }}</h2>
				<div class="desc">
					<v-star
						:size="36"
						:score="seller.score">
					</v-star>
					<span class="text">({{ seller.ratingCount }})</span>
					<span class="text">月售{{ seller.sellCount }}单</span>
				</div>
				<ul class="remark">
					<li class="block">
						<h3>起送价</h3>
						<div class="content">
							<span class="stress">{{ seller.minPrice }}</span>元
						</div>
					</li>
					<li class="block">
						<h3>商家配送</h3>
						<div class="content">
							<span class="stress">{{ seller.deliveryPrice }}</span>
						</div>
					</li>
					<li class="block">
						<h3>平均配送时间</h3>
						<div class="content">
							<span class="stress">{{ seller.deliveryTime }}</span>分钟
						</div>
					</li>
				</ul>
			</div>
			<v-split></v-split>
			<div class="bulletin">
				<div class="title">公告与活动</div>
				<div class="content-wrapper">
					<p class="content">{{ seller.bulletin }}</p>
				</div>
				<ul class="supports"
					v-if="seller.supports">
					<li class="support-item"
						v-for="(item, index) of seller.supports"
						:key="index">
						<v-icon
							:num="4"
							:type="item.type">
						</v-icon>
						<span class="text">{{ item.description }}</span>
					</li>
				</ul>
			</div>
			<v-split></v-split>
			<div class="pics" style="transform: translate3d(0, 0, 0)">
				<h2 class="title">商家实景</h2>
				<div class="pic-wrapper" ref="picWrapper">
					<ul class="pic-list" ref="picList">
						<li class="pic-item"
							v-for="(pic, index) of seller.pics"
							:key="index">
							<img :src="pic" width="120" height="90"/>
						</li>
					</ul>
				</div>
				<v-preview
					:images="seller.picList">
				</v-preview>
			</div>
		</div>
	</div>
</template>
<script>
import BScroll from 'better-scroll';
import star from '@/components/star/star';
import split from '@/components/split/split.vue';
import icon from '@/components/icon/icon.vue';
import preview from '@/components/preview/preview.vue';
export default {
	name: 'seller',
	components: {
		'v-star': star,
		'v-split': split,
		'v-icon': icon,
		'v-preview': preview,
	},
	props: {
		seller: {
			required: true,
			type: Object,
		}
	},
	watch: {
		seller() {
			this.$nextTick(() => {
				this.$_initScroll();
				this.$_initPics();
			});
		}
	},
	mounted() {
		this.$nextTick(() => {
			this.$_initScroll();
			this.$_initPics();
		});
	},
	methods: {
		$_initScroll() {
			if (!this.scroll) {
				this.scroll = new BScroll(this.$refs.seller, {
					click: true,
				});
			} else {
				this.scroll.refresh();
			}
		},
		$_initPics() {
			if (this.seller.pics) {
				let picWidth = 120;
				let margin = 6;
				let width = (picWidth + margin) * this.seller.pics.length - margin;
				this.$refs.picList.style.width = width + 'px';
				this.$nextTick(() => {
					if (!this.picScroll) {
						this.picScroll = new BScroll(this.$refs.picWrapper, {
							scrollX: true,
							eventPassthrought: 'vertical',
						});
					} else {
						this.picScroll.refresh();
					}
				});
			}
		}
	},

}
</script>
<style lang="less" scoped>
@import "../../common/style/mixin.less";
* {
	touch-action: pan-y;
}
.seller {
	position: absolute;
	top: 174px;
	bottom: 0;
	left: 0;
	right: 0;
	overflow: hidden;
	.overview {
		padding: 18px;
		
		.title {
			margin-bottom: 8px;
			font-size: 14px;
			line-height: 14px;
			color: rgb(7, 17, 27);
		}

		.desc {
			padding-bottom: 18px;
			font-size: 0;
			.border-1px(rgba(7, 17, 27, 0.1));
			.star {
				display: inline-block;
				margin-right: 8px;
				vertical-align: top;
			}
			.text {
				margin-right: 8px;
				font-size: 10px;
				line-height: 18px;
				vertical-align: top;
				color: rgb(77, 85, 93);
			}
		}

		.remark {
			display: flex;
			padding-top: 18px;
			
			.block {
				flex: 1;
				text-align: center;
				border-right: 1px solid rgba(7, 17, 27, 0.1);
				
				&:last-child {
					border: none;
				}

				h3 {
					margin-bottom: 4px;
					font-size: 10px;
					line-height: 10px;
					color: rgb(147, 153, 159);
				}

				.content {
					line-height: 24px;
					font-size: 10px;
					color: rgb(7, 17, 27);

					.stress {
						font-size: 24px;
					}
				}
			}
		}
	}

	.bulletin {
		padding: 18px 18px 0 18px;

		.title {
			margin-bottom: 8px;
			line-height: 14px;
			color: rgb(7, 17, 27);
			font-size: 14px;
		}

		.content-wrapper {
			line-height: 24px;
			font-size: 12px;
			color: rgb(240, 20, 20);
		}

		.supports {

			.support-item {
				padding: 16px 12px;
				.border-1px(rgba(7, 17, 27, 0.1));
				font-size: 0;
				&:last-child {
					.border-none();
				}
			}

			.icon {
				display: inline-block;
				width: 16px;
				height: 16px;
				vertical-align: top;
				margin-right: 6px;
				background-size: 16px 16px;
				background-repeat: no-repeat;
			}

			.text {
				font-size: 12px;
				line-height: 16px;
				color: rgb(7, 17, 27);
			}
		}
	}

	.pics {
		background-color: green;
		padding: 18px;

		.title {
			background-color: skyblue;
			margin-bottom: 12px;
			font-size: 14px;
			color: rgb(7, 17, 27);
			line-height: 14px;
		}

		.pic-wrapper {
			background-color: pink;
			width: 100%;
			overflow: hidden;
			white-space: nowrap;

			.pic-list {
				background-color: purple;
				font-size: 0;

				.pic-item {
					background-color: aqua;
					display: inline-block;
					margin-right: 6px;
					width: 120px;
					height: 90px;
					
					&:last-child {
						background-color: gold;
						margin: 0;
					}
				}
			}
		}
	}
}
</style>