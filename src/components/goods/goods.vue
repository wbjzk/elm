<template>
	<div class="goods">
		<div class="menu-wrapper">
			<ul>
				<li class="menu-item" v-for="(item, i) in goods" :key="i">
					<span class="text border-1px">
						<v-icon v-show="item.type > 0" :type="item.type" :num="3"></v-icon>{{ item.name }}
					</span>
				</li>
			</ul>
		</div>
		<div class="foods-wrapper"></div>
	</div>
</template>
<script>
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
			}
		});
	},
	components: {
		'v-icon': icon
	}
};
</script>
<style lang="less" scoped>
@import "../../common/style/mixin.less";
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
	}
}
</style>