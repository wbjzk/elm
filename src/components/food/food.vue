<template>
    <transition>
        <div v-show="showFlag" class="food" ref="food">
            <div class="food-content">

                <!-- 详情页头部 start -->
                <div class="image-header">
                    <img :src="food.image"/>
                    <div class="back" @click.stop="hide">
                        <i class="icon-arrow_lift"></i>
                    </div>
                </div>
                <!-- 详情页头部 start -->

                <!-- 详情页内容 start -->
                <div class="content">

                    <!-- 食物名称 -->
                    <div class="title">{{ food.name }}</div>

                    <!-- 食物的详细介绍 -->
                    <div class="detail">
                        <span class="sell-count">月售{{ food.sellCount }}份</span>
                        <span class="rating">好评率{{ food.rating }}%</span>
                    </div>

                    <!-- 食物价格 -->
                    <div class="price">
                        <span class="now">￥{{ food.price }}</span><span class="old" v-show="food.oldPrice">￥{{ food.oldPrice }}</span>
                    </div>

                    <!-- 数量控制器 start -->
                    <div class="cartcontrol-wrapper">
                        <v-cartcontrol :food="food" @add="drop"></v-cartcontrol>
                    </div>
                    <!-- 数量控制器 end -->

                    <!-- 加入购物车按钮 start -->
                    <div class="buy" v-show="!food.count || food.count === 0" @click="addFirst">加入购物车</div>
                    <!-- 加入购物车按钮 end -->
                </div>
                <!-- 详情页内容 end -->
                <v-split></v-split>
            </div>
        </div>
    </transition>
</template>
<script>
import BScroll from 'better-scroll';
import cartcontrol from '../cartcontrol/cartcontrol.vue';
import split from '../split/split.vue';
export default {
    props: {
        food: {
            type: Object,
        },
    },
    data() {
        return {
            showFlag: false
        };
    },
    methods: {
        show() {
            this.showFlag = true;
            // 详情页显示的时候设置可以单击且可以滚动
            this.$nextTick(() => {
                if(!this.scroll) {
                    this.scroll = new BScroll(this.$refs.food, {
                        click: true,
                    });
                } else {
                    this.scroll.refresh();
                }
            });
        },
        hide() {
            this.showFlag = false;
        },
        addFirst(event) {
            if(!event._constructed) {
                return;
            }
            this.$emit('drop', event.target);
            this.$set(this.food, 'count', 1);
        },
        drop(target) {
            this.$emit('drop', event.target);
        }
    },
    components: {
        'v-cartcontrol': cartcontrol,
        'v-split': split,
    }
};
</script>
<style lang="less" scoped>
.food {
    position: fixed;
    left: 0;
    top: 0;
    bottom: 48px;
    z-index: 30;
    width: 100%;
    background: #fff;
    &.v-enter {
        transform: translate3d(100%, 0, 0);
    }
    &.v-leave-to {
        transform: translated3d(-100%, 0, 0);
    }
    &.v-enter-active,
    &.v-leave-active {
        transition: all .5s;
    }

    .food-content {
        .image-header {
            position: relative;
            width: 100%;
            height: 0;
            padding-top: 100%;
            img {
                position: absolute;
                top: 0;
                left: 0;
                width: 100%;
                height: 100%;
                background-color: #000;
            }
            .back {
                position: absolute;
                top: 10px;
                left: 0;
                .icon-arrow_lift {
                    display: block;
                    padding: 10px;
                    font-size: 20px;
                    color: #fff;
                }
            }
        }
        .content {
            position: relative;
            padding: 10px;
            .title {
                line-height: 14px;
                margin-bottom: 8px;
                font-size: 14px;
                font-weight: bold;
                color: rgb(7, 17, 27);
            }
            .detail {
                margin-bottom: 18px;
                line-height: 10px;
                height: 10px;
                font-size: 0;
                .sell-count,
                .rating {
                    font-size: 10px;
                    color: rgb(147, 153, 159);
                }
                .sell-count {
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
                right: 12px;
                bottom: 12px;
            }
            .buy {
                position: absolute;
                right: 18px;
                bottom: 18px;
                z-index: 10;
                height: 24px;
                line-height: 24px;
                padding: 0 12px;
                box-sizing: border-box;
                border-radius: 12px;
                font-size: 10px;
                background: rgb(0, 160, 220);
            }
        }
    }
}
</style>
