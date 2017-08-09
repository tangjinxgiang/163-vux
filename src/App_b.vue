<template>
  <div id="app"> <!--网易新闻-->
    <view-box>
      <x-header class="header" :left-options="{showBack:false}">
        <div slot="left">返回</div>
        <div slot="default">网易</div>
        <div slot="right">搜索</div>
      </x-header>
      <scroller :lock-y="true">
        <div class="tab">
          <tab>
            <tab-item>推荐</tab-item>
            <tab-item>推荐</tab-item>
            <tab-item>推荐</tab-item>
            <tab-item>推荐</tab-item>
            <tab-item>推荐</tab-item>
            <tab-item>推荐</tab-item>
          </tab>
        </div>
      </scroller>

      <swiper :list="baseList" v-model="baseIndex" loop auto></swiper>

      <marquee>
        <marquee-item>111</marquee-item>
        <marquee-item>222</marquee-item>
        <marquee-item>333</marquee-item>
      </marquee>

      <panel :list="list"></panel>

      <tabbar>
        <tabbar-item selected>
          <img slot="icon" src="./assets/icon_nav_msg.png" alt="">
          <div slot="label">首页</div>
        </tabbar-item>
        <tabbar-item>
          <img slot="icon" src="./assets/icon_nav_msg.png" alt="">
          <div slot="label">首页</div>
        </tabbar-item>
        <tabbar-item>
          <img slot="icon" src="./assets/icon_nav_msg.png" alt="">
          <div slot="label">首页</div>
        </tabbar-item>
        <tabbar-item>
          <img slot="icon" src="./assets/icon_nav_msg.png" alt="">
          <div slot="label">首页</div>
        </tabbar-item>
      </tabbar>
    </view-box>
    <router-view></router-view>
  </div>
</template>
<script type="text/ecmascript-6">
    import {
      ViewBox,
      XHeader ,
      Tab,
      TabItem,
      Scroller,
      Swiper,
      Marquee,
      MarqueeItem,
      Panel,
      Tabbar,
      TabbarItem
    } from 'vux'
    export default {
      name: "app",
      components:{
        ViewBox,
        XHeader,
        Tab,
        TabItem,
        Scroller,
        Swiper,
        Marquee,
        MarqueeItem,
        Panel,
        Tabbar,
        TabbarItem
      },
      data(){
          return{
            baseList : [{
              url: 'javascript:',
              img: 'https://static.vux.li/demo/1.jpg',
              title: '送你一朵fua'
            }, {
              url: 'javascript:',
              img: 'https://static.vux.li/demo/2.jpg',
              title: '送你一辆车'
            }, {
              url: 'javascript:',
              img: 'https://static.vux.li/demo/3.jpg',
              title: '送你一次旅行'
            }],
            baseIndex:2,
            type: '1',
            list: [{
              src: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
              title: '标题一',
              desc: '由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。',
              url: '/component/cell'
            },{
              src: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
              title: '标题二',
              desc: '由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。',
              url: {
                path: '/component/radio',
                replace: false
              }
            },
              {
                src: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
                title: '标题二',
                desc: '由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。',
                url: {
                  path: '/component/radio',
                  replace: false
                }
              },
              {
                src: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
                title: '标题二',
                desc: '由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。',
                url: {
                  path: '/component/radio',
                  replace: false
                }
              },
              {
                src: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
                title: '标题二',
                desc: '由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。',
                url: {
                  path: '/component/radio',
                  replace: false
                }
              }]
          }
      }
    }
</script>
<style lang="less">
  @import '~vux/src/styles/reset.less';
  html, body {
    height: 100%;
    width: 100%;
    overflow-x: hidden;
  }
  #app{
    height:100%;
    .header{
      position: fixed;
      top:0;
      left: 0;
      width:100%;
      z-index: 10000;
    }
    .tab{
      width:1200px;
      margin-top:46px;
    }
  }
</style>
