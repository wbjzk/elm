<template>
	<div class="header">
		<div class="content-wrapper">
			<div class="avatar">
				<img :src="seller.avatar" width="64" height="64">
			</div>
			<div class="content">
				<div class="title">
					<span class="brand"></span>
					<span class="name">{{ seller.name }}</span>
				</div>
				<div class="description">
					{{ seller.description }}/{{ seller.deliveryTime }}分钟送达
				</div>
				<div class="support" v-if="seller.supports">
					<!-- <span class="icon" :class="classMap[seller.supports[0].type]"></span> -->
					<v-icon :type="seller.supports[0].type" :num="1"></v-icon>
					<span class="text">{{ seller.supports[0].description }}</span>
				</div>
			</div>
			<div class="support-count" @click="toggleDetail">
				<span class="count" v-if="seller.supports">{{ seller.supports.length }}个</span>
				<i class="icon-keyboard_arrow_right"></i>
			</div>
		</div>
		<div class="bulletin-wrapper" @click="toggleDetail">
			<span class="bulletin-title"></span><span class="bulletin-text">{{ seller.bulletin }}</span><i class="icon-keyboard_arrow_right"></i>
		</div>
		<div class="background">
			<img :src="seller.avatar" width="100%" height="100%">
		</div>
		<transition name="fade">
			<div class="detail" v-show="detailShow">
				<div class="detail-warapper clearfix">
					<div class="detail-main">
						<h2 class="name">{{ seller.name }}</h2>
						<div class="star-wrapper">
							<star :size="48" :score="seller.score"></star>
						</div>
						<div is="v-line" :title="'优惠信息'"></div>
						<ul class="supports" v-if="seller.supports">
							<li class="support-item" v-for="(item, i) in seller.supports" :key="i">
								<v-icon :type="seller.supports[i].type" :num="2"></v-icon>
								<span class="text">{{ seller.supports[i].description }}</span>
							</li>
						</ul>
						<div is="v-line" :title="'商家公告'"></div>
						<div class="bulletin">
							<p class="content">{{ seller.bulletin }}</p>
						</div>
					</div>
				</div>
				<div class="detail-close" @click="toggleDetail">
					<i class="icon-close"></i>
				</div>
			</div>
		</transition>
	</div>
</template>

<script>
import Star from '@/components/star/star';
import Line from '@/components/line/line';
import Icon from '@/components/icon/icon';
export default {
	created() {
		console.log(JSON.stringify(this.seller.supports, null, 4));
	},
	data() {
		return {
			classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
			detailShow: false
		};
	},
	props: {
		seller: {
			required: true,
			type: Object,
		}
	},
	methods: {
		toggleDetail() {
			this.detailShow = !this.detailShow;
		}
	},
	components: {
		Star,
		'v-line': Line,
		'v-icon': Icon,
	}
}
</script>
<style lang="less">
@import "../../common/style/mixin.less";
.header {
	position: relative;
	color: #fff;
	overflow: hidden;
	background: rgba(7,17,27,.5);
	.content-wrapper {
		position: relative;
		padding: 24px 12px 18px 24px;
		overflow: hidden;
		font-size: 0;
		*ord-spacing: -1px;
		.avatar {
			float: left;
			img {
				vertical-align: bottom;
				border-radius: 2px;
			}
		}
		.content {
			float: left;
			margin-left: 16px;
			.title {
				margin: 2px 0 8px 0;
				.brand {
					display: inline-block;
					width: 30px;
					height: 18px;
					.bg-image('brand');
					background-size: 30px 18px;
					background-repeat: no-repeat;
					vertical-align: top;
				}
				.name {
					margin-left: 6px;
					font-size: 16px;
					line-height: 18px;
					font-weight: bold;
				}
			}
			.description {
				margin-bottom: 10px;
				line-height: 12px;
				font-size: 12px;
			}
			.support {
				.icon {
					display: inline-block;
					vertical-align: top;
					width: 12px;
					height: 12px;
					margin-right: 4px;
					background-size: 12px 12px;
					background-repeat: no-repeat;
					// &.decrease {
					// 	.bg-image('decrease_1');
					// }
					// &.discount {
					// 	.bg-image('discount_1');
					// }
					// &.guarantee {
					// 	.bg-image('guarantee_1');
					// }
					// &.invoice {
					// 	.bg-image('invoice_1');
					// }
					// &.special {
					// 	.bg-image('special_1');
					// }
				}
				.text {
					line-height: 12px;
					font-size: 12px;
				}
			}
		}
		.support-count {
            position: absolute;
            right: 12px;
            bottom: 14px;
            padding: 0 8px;
            height: 24px;
            line-height: 24px;
            border-radius: 14px;
            background: rgba(0, 0, 0, .2);
            text-align: center;
            .count {
                vertical-align: top;
                font-size: 10px;
            }
            .icon-keyboard_arrow_right {
                margin-left: 2px;
                line-height: 24px;
                font-size: 10px;
            }
        }
	}
	.bulletin-wrapper {
		position: relative;
		height: 28px;
		line-height: 28px;
		padding: 0 22px 0 12px;
		white-space: nowrap; //禁止换行
		overflow: hidden;
		text-overflow: ellipsis; //超出部分省略
		background: rgba(7,17,27,.2);
		.bulletin-title {
			display: inline-block;
			vertical-align: top;
			margin-top: 8px;
			width: 22px;
			height: 12px;
			.bg-image('bulletin');
			background-size: 22px 12px;
			background-repeat: no-repeat;
		}
		.bulletin-text {
			vertical-align: top;
			margin: 0 4px;
			font-size: 10px;
		}
		.icon-keyboard_arrow_right {
			position: absolute;
			font-size: 10px;
			right: 12px;
			top: 8px;
		}
	}
	.background {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		z-index: -1;
		filter: blur(10px);
	}
	.detail {
		position: fixed;
		z-index: 100;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		overflow: auto;
		background: rgba(7,17,27,.8);
		&.fade-enter-active,
		&.fade-leave-active {
			transition: opacity 1S;
		}
		&.fade-enter,
		&.fade-leave-to {
			opacity: 0;
		}
		.detail-warapper {
			width: 100%;
			min-height: 100%;
			.detail-main {
				margin-top: 64px;
				padding-bottom: 64px;
				.name {
					line-height: 16px;
					text-align: center;
					font-size: 16px;
					font-weight: bold;
				}
				.star-wrapper {
					margin-top: 18px;
					padding: 2px 0;
					text-align: center;
				}
				.supports {
					width: 80%;
					margin: 0 auto;
					.support-item {
						padding: 0 12px;
						margin-bottom: 12px;
						font-size: 0;
						&:last-child {
							margin-bottom: 0;
						}
						.icon {
							display: inline-block;
							width: 16px;
							height: 16px;
							vertical-align: top;
							margin-right: 6px;
							background-size: 16px 16px;
							background-repeat: no-repeat;
							// &.decrease {
							// 	.bg-image('decrease_2');
							// }
							// &.discount {
							// 	.bg-image('discount_2');
							// }
							// &.guarantee {
							// 	.bg-image('guarantee_2');
							// }
							// &.invoice {
							// 	.bg-image('invoice_2');
							// }
							// &.special {
							// 	.bg-image('special_2');
							// }
						}
						.text {
							line-height: 16px;
							font-size: 12px;
						}
					}
				}
				.bulletin {
					width: 80%;
					margin: 0 auto;
					.conent {
						padding: 0 12px;
						line-height: 24px;
						font-size: 12px;
					}
				}
			}
		}
		.detail-close {
			// background: #ccc;
			position: relative;
			width: 32px;
			height: 32px;
			margin: -64px auto 0 auto;
			clear: both;
			font-size: 32px;
			.icon-close {

			}
		}
	}
}
</style>