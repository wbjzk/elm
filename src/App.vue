<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab">
      <div class="tab-item"><router-link :to="{ name: 'goods' }">商品</router-link></div>
      <div class="tab-item"><router-link :to="{ name: 'ratings' }">评论</router-link></div>
      <div class="tab-item"><router-link :to="{ name: 'seller' }">商家</router-link></div>
    </div>
    <router-view/>
  </div>
</template>

<script>
import header from '@/components/header/header.vue';
export default {
  name: 'App',
  data() {
    return {
      seller: {},
    };
  },
  created() {
    this.getSeller();
  },
  methods: {
    getSeller() {
      this.axios.get('api/seller').then(res => {
        if(res.data.errno === 0) {
          this.seller = res.data.seller;
        }
      }).catch(error => {
        console.log(error);
      });
    }
  },
  components: {
    'v-header': header
  }
}
</script>

<style lang="less">
@import './common/style/mixin.less';
// #app {
//   font-family: 'Avenir', Helvetica, Arial, sans-serif;
//   -webkit-font-smoothing: antialiased;
//   -moz-osx-font-smoothing: grayscale;
//   text-align: center;
//   color: #2c3e50;
//   margin-top: 60px;
// }
.tab {
  display: flex;
  width: 100%;
  height: 40px;
  line-height: 40px;
  .border-1px(rgba(7,17,27,.1));
  .tab-item {
    flex: 1;
    text-align: center;
    & > a {
      display: block;
      font-size: 14px;
      color: rgb(77,85,93);
      &.active {
        color: rgb(240,20,20);
      }
    }
  }
}
</style>
