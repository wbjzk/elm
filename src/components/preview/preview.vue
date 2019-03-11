<template>
  <vue-preview class="thumbs"
    ref="thumbs"
    tag="div"
    :slides="images"
    @close="handleClose">
  </vue-preview>
</template>
<script>
import BScroll from 'better-scroll';
export default {
  props: {
    images: {
      type: Array,
    }
  },
  // 事件（通过响应式事件触发的回调）
  watch: {
    images(newVal, oldVal) {
      this.$nextTick(() => {
        this.$_initPics();
      });
    }
  },
  mounted() {
    this.$_initPics();
    this.$nextTick(() => {
      this.$_initPics();
    });
  },
  methods: {
    $_initPics() {
      if (this.images) {
        let picWidth = 120;
        let margin = 6;
        let width = (picWidth + margin) * this.images.length - margin;
        document.getElementsByClassName('my-gallery')[0].style.width = width + 'px';
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(document.getElementsByClassName('thumbs')[0], {
              scrollX: true,
              eventPassthrought: 'vertical',
            });
          } else {
            this.scroll.refresh();
          }
        });
      }
    },
    handleClose() {
      console.log("close event");
    }
  }
}
</script>
<style lang="less">
.thumbs {
  display: block;
  height: 100px;
  white-space: nowrap;
  overflow: hidden;

  .my-gallery {
    figure {
      display: inline-block;
      margin-right: 6px;

      img {
        display: inline-block;
        width: 120px;
        height: 90px;

        &:last-child {
          margin: 0;
        }
      }
    }
  }
}
</style>