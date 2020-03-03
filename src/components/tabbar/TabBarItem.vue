<template>
  <div class="tab-bar-item" @click="tabBarItemClick">
    <div v-if="!isActive">
      <slot name="item-icon"></slot>
    </div>
    <div v-else>
      <slot name="item-icon-active"></slot>
    </div>
<!--    通过动态获取颜色样式来进行封装-->
    <div :style="activeStyle">
      <slot name="item-text"></slot>
    </div>
  </div>
</template>

<script>
  export default {
    name: "TabBarItem",
    data() {
      return {
        // isActive:true
      }
    },
    //传入在App.vue中对应属性，
    props: {
      path: String,
      activeColor: {type: String, default: "#409EFF"}
    },
    computed: {
      isActive() {
        //indexOf函数，判断路由中的path与indexOf里面的path如果不一致返回-1，保证
        return this.$route.path.indexOf(this.path) !== -1
      },
      activeStyle() {
        //计算属性动态获取用户输入的tabbar颜色，完成封装
        return this.isActive ? {color: this.activeColor} : {}
      }
    },
    methods: {
      //通过router获得对应的url
      tabBarItemClick () {
        //通过后面的报错机制，来避免双击路由报错
        this.$router.replace(this.path, () => {
        }, (e) => {
          console.log('输出报错', e)
        })
      },
    }
  }
</script>

<style scoped>
  .tab-bar-item {
    flex: 5;
    text-align: center;
    height: 49px;
    font-size: 14px;
  }

  .tab-bar-item img {
    width: 24px;
    height: 24px;
    margin-top: 3px;
    margin-bottom: 2px;
    vertical-align: middle;
  }

  /*.active {
    color: #409EFF;
  }*/
</style>