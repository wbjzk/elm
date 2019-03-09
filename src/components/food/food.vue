<template>
  <transition>
    <div v-show="showFlag" class="food" ref="food">
      <div class="food-content">

        <!-- 详情页头部 start -->
        <div class="image-header">
          <img :src="food.image">
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
            <span class="now">￥{{ food.price }}</span>
            <span class="old" v-show="food.oldPrice">￥{{ food.oldPrice }}</span>
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

        <!-- 分隔间隙 start -->
        <v-split></v-split>
        <!-- 分隔间隙 end -->

        <!-- 商品信息 start -->
        <div class="info" v-show="food.info">
          <div class="title">商品信息</div>
          <p class="text">{{ food.info }}</p>
        </div>
        <!-- 商品信息 end -->

        <!-- 分隔间隙 start -->
        <v-split></v-split>
        <!-- 分隔间隙 end -->

        <!-- 商品评价 start -->
        <div class="rating">
          <h2 class="title">商品评价</h2>
          <v-ratingselect
            @select="select"
            @toggle="toggleContent"
            :select-type="selectType"
            :only-content="onlyContent"
            :desc="desc"
            :ratings="food.ratings"
          ></v-ratingselect>
          <div class="rating-wrapper">
            <ul v-show="food.ratings && food.ratings.length">
              <li class="rating-item" v-for="rating in food.ratings" v-show="needShow(rating.rateType, rating.text)" :key="rating.username">
                <div class="user">
                  <span class="name">{{ rating.username }}</span>
                  <img class="avatar" width="12" height="12" :src="rating.avatar">
                </div>
                <div class="time">{{ rating.rateTime | formatDate }}</div>
                <p class="text">
                  <span
                    :class="{ 'icon-thumb_up': rating.rateType === 0 , 'icon-thumb_down': rating.rateType === 1 }"
                  ></span>
                  {{ rating.text }}
                </p>
              </li>
            </ul>
            <div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评论</div>
          </div>
        </div>
        <!-- 商品评价 end -->

      </div>
    </div>
  </transition>
</template>
<script>
import BScroll from "better-scroll";
import cartcontrol from "../cartcontrol/cartcontrol.vue";
import split from "../split/split.vue";
import ratingselect from "../ratingselect/ratingselect.vue";
import { formatDate } from "@/common/js/date.js";

const POSITIVE = 0;
const NEGATIVE = 1;
const ALL = 2;

export default {
  components: {
    "v-cartcontrol": cartcontrol,
    "v-split": split,
    "v-ratingselect": ratingselect
  },
  filters: {
    formatDate(time) {
      let date = new Date(time);
      return formatDate(date, 'yyyy-MM-dd hh:m');
    }
  },
  props: {
    food: {
      type: Object
    }
  },
  data() {
    return {
      showFlag: false,
      selectType: ALL, // 商品评价：全部-All、满意-NEGATIVE、不满意-POSITIVE
      onlyContent: true,
      desc: {
        all: "全部",
        positive: "推荐",
        negative: "吐槽"
      }
    };
  },
  methods: {
    show() {
      this.showFlag = true;
      // 详情页显示的时候设置可以单击且可以滚动
      this.selectType = ALL;
      this.onlyContent = true;
      this.$nextTick(() => {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.food, {
            click: true
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
      if (!event._constructed) {
        return;
      }
      this.$emit("drop", event.target);
      this.$set(this.food, "count", 1);
    },
    drop(target) {
      this.$emit("drop", event.target);
    },
    select(obj) {
      if (!obj.event._constructed) {
        return;
      }
      this.selectType = obj.type;
      this.$nextTick(() => {
        this.scroll.refresh();
      });
    },
    toggleContent(event) {
      // 排除原生单击事件
      if (!event._constructed) {
        return;
      }
      this.onlyContent = !this.onlyContent;
      // 将回调延迟到下次DOM更新循环之后执行
      this.$nextTick(() => {
        this.scroll.refresh();
      });
    },
    needShow(type, text) {
      if (this.onlyContent && !text) {
        return false;
      }
      if (this.selectType === ALL) {
        return true;
      } else {
        return type === this.selectType;
      }
    }
  }
};
</script>
<style lang="less" scoped>
@import "../../common/style/mixin.less";
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
    transition: all 0.5s;
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
    .info {
      padding: 18px;
      .title {
        line-height: 14px;
        margin-bottom: 6px;
        font-size: 14px;
        color: rgb(7, 17, 27);
      }
      .text {
        line-height: 24px;
        padding: 0 8px;
        font-size: 12px;
        color: rgb(77, 85, 93);
      }
    }
    > .rating {
      padding-top: 18px;
      > .title {
        line-height: 14px;
        margin-left: 18px;
        font-size: 14px;
        color: rgb(7, 17, 27);
      }
      > .rating-wrapper {
        padding: 0 18px;
        .rating-item {
          position: relative;
          padding: 16px 0;
          .border-1px(rgba(7, 17, 27, 0.1));
          > .user {
            position: absolute;
            right: 0;
            top: 16px;
            line-height: 12px;
            font-size: 0;
            > .name {
              display: inline-block;
              margin-right: 6px;
              vertical-align: top;
              font-size: 10px;
              color: rgb(147, 153, 159);
            }
            > .avatar {
              border-radius: 50%;
            }
          }
          .time {
            margin-bottom: 6px;
            line-height: 12px;
            font-size: 10px;
            color: rgb(147, 153, 159);
          }
          .text {
            line-height: 16px;
            font-size: 12px;
            color: rgb(7, 17, 27);
            > .icon-thumb_up,
            > .icon-thumb_down {
              margin-right: 4px;
              line-height: 16px;
              font-size: 12px;
            }
            > .icon-thumb_up {
              color: rgb(0, 160, 220);
            }
            > .icon-thumb_down {
              color: rgb(147, 153, 159);
            }
          }
        }
        .no-rating {
            padding: 16px 0;
            font-size: 12px;
            color: rgb(147,153,159);
        }
      }
    }
  }
}
</style>
