<template>
	<span class="icon" :class="iconStyle"></span>
</template>
<script>
export default {
	data() {
		return {
			classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee']
		};
	},
	props: {
		type: {
			required: true,
			type: Number,
		},
		num: {
			required: true,
			type: Number,
		}
	},
	computed: {
		iconStyle() {
			return this.classMap[this.type]+'-'+this.num;
		}
	}
}
</script>
<style lang="less" scoped>
@import "../../common/style/mixin.less";
.decrease-1 {
	.bg-image('decrease_1');
}

.discount-1 {
	.bg-image('discount_1');
}

.gurantee-1 {
	.bg-image('guarantee_1');
}

.invoice-1 {
	.bg-image('invoice_1');
}

.special-1 {
	.bg-image('special_1');
}

.decrease-2 {
	.bg-image('decrease_2');
}

.discount-2 {
	.bg-image('discount_2');
}

.gurantee-2 {
	.bg-image('guarantee_2');
}

.invoice-2 {
	.bg-image('invoice_2');
}

.special-2 {
	.bg-image('special_2');
}
.decrease-3 {
	.bg-image('decrease_3');
}

.discount-3 {
	.bg-image('discount_3');
}

.gurantee-3 {
	.bg-image('guarantee_3');
}

.invoice-3 {
	.bg-image('invoice_3');
}

.special-3 {
	.bg-image('special_3');
}
</style>