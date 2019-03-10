<template>
	<div class="seller">
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
		</div>
	</div>
</template>
<script>
import star from '@/components/star/star';
import split from '@/components/split/split.vue';
export default {
	name: 'seller',
	components: {
		'v-star': star,
		'v-split': split,
	},
	props: {
		seller: {
			required: true,
			type: Object,
		}
	}
}
</script>
<style lang="less" scoped>
@import "../../common/style/mixin.less";
.seller {
	background-color: aqua;
	position: absolute;
	top: 174px;
	bottom: 0;
	left: 0;
	right: 0;
	overflow: hidden;
	.overview {
		background-color: antiquewhite;
		padding: 18px;
		
		.title {
			background-color: aquamarine;
			margin-bottom: 8px;
			font-size: 14px;
			line-height: 14px;
			color: rgb(7, 17, 27);
		}

		.desc {
			background-color: blueviolet;
			padding-bottom: 18px;
			font-size: 0;
			.border-1px(rgba(7, 17, 27, 0.1));
			.star {
				background-color: brown;
				display: inline-block;
				margin-right: 8px;
				vertical-align: top;
			}
			.text {
				background-color: skyblue;
				margin-right: 8px;
				font-size: 10px;
				line-height: 18px;
				vertical-align: top;
				color: rgb(77, 85, 93);
			}
		}

		.remark {
			background-color: pink;
			display: flex;
			padding-top: 18px;
			
			.block {
				background-color: hotpink;
				flex: 1;
				text-align: center;
				border-right: 1px solid rgba(7, 17, 27, 0.1);
				
				&:last-child {
					background-color: red;
					border: none;
				}

				h3 {
					background-color: skyblue;
					margin-bottom: 4px;
					font-size: 10px;
					line-height: 10px;
					color: rgb(147, 153, 159);
				}

				.content {
					background-color: greenyellow;
					line-height: 24px;
					font-size: 10px;
					color: rgb(7, 17, 27);

					.stress {
						background-color: yellowgreen;
						font-size: 24px;
					}
				}
			}
		}
	}

	.bulletin {
		background-color: purple;
		padding: 18px 18px 0 18px;

		.title {
			background-color: hotpink;
			margin-bottom: 8px;
			line-height: 14px;
			color: rgb(7, 17, 27);
			font-size: 14px;
		}

		.content-wrapper {
			background-color: beige;
			line-height: 24px;
			font-size: 12px;
			color: rgb(240, 20, 20);
		}
	}
}
</style>