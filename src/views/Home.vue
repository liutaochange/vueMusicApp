<template>
  <div>
    <div class="fixed-bar">
      <mu-appbar>
        <div class="logo" slot="left"></div>
        <mu-icon-button icon='search'  slot="right"/>
      </mu-appbar>
      <mu-tabs :value="activeTab" @change="handleTabChange" class="view-tabs">
        <mu-tab value="recommend" title="个性推荐"/>
        <mu-tab value="songList" title="歌单"/>
        <mu-tab value="host" title="主播电台"/>
        <mu-tab value="rank" title="排行榜"/>
      </mu-tabs>
    </div>
    <div class="default-view">
      <first :is="activeTab" keep-alive></first>
    </div>
  </div>
</template>
<script>
  // 导入子组件
  import recommend from './recommend'
  import songList from './songList'
  import host from './host'
  import rank from './rank'
  export default {
    data () {
      return {
        activeTab: '' // 默认打开第一个  个性推荐
      }
    },
    components: {
      recommend,
      songList,
      host,
      rank
    },
    created () {
      const hIndex = this.$store.state.home_index
      this.activeTab = hIndex
      // created函数 监测路由信息,防止页面刷新tab高亮错误
      var tmpArr = this.$route.path.split('/')
      if (tmpArr[1] === 'index') {
        this.handleTabChange(hIndex)
      }
    },
    // watch函数监测路由的变化,保持tab面板的高亮位置正确
    watch: {
      '$route' (to, from) {
        const path = to.path
        var tmpArr = path.split('/')
        if (tmpArr[1] === 'index') {
          this.handleTabChange(tmpArr[2])
        }
      }
    },
    methods: {
      handleTabChange (val) {
        this.activeTab = val
        this.$store.commit('setIndex', val)
        this.$router.push({ path: '/index/' + val })
      }
    }
  }
</script>
<style lang="less" scoped>
  @import "../assets/theme.less";
  .logo {
    width: 150px;
    height: 48px;
    background: url("../../static/images/logo.png") no-repeat left center;
    background-size: cover;
  }
  .view-tabs {
    background-color: #fff;
    color: rgba(0,0,0,.87);
    &>.mu-tab-link {
       color: rgba(102,102,102,1);
     }
    &>.mu-tab-active{
       color: @primaryColor;
     }
  }
  .fixed-bar {
    position: fixed;
    width: 100%;
    top:0;
    left: 0;
    z-index: 15;
  }
  .default-view {
    margin-top: 104px;
    padding-bottom: 56px;
  }
</style>

