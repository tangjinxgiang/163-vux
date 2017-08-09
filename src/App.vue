<template>
  <div id="app"> <!--网易新闻-->
    <view-box>
      <x-header
        class="header"
        slot="header"
        :left-options="{showBack:false}"
      >
        <div slot="left">直播</div>
        <div slot="default">网易</div>
        <div slot="right">搜索</div>
      </x-header>
      <Sc :lock-y="true">
        <div class="tab">
          <tab>
            <tab-item selected>推荐</tab-item>
            <tab-item>视频</tab-item>
            <tab-item>热点</tab-item>
            <tab-item>直播</tab-item>
            <tab-item>关注</tab-item>
            <tab-item>怀化</tab-item>
          </tab>
        </div>
      </Sc>
     <scroller class="my-scroller"
               :on-refresh="refresh"
               :on-infinite="infinite"
               refresh-text="刷新lol"
                ref="myRef"
     >
       <swiper :list="swiperList" v-model="swiperIndex" :loop="true"></swiper>

       <marquee direction="buttom" class="my-marquee">
         <marquee-item v-for="list in marqueeList">{{list.title}}</marquee-item>
       </marquee>

       <panel :list="datalist"></panel>
       <panel :list="moreDataList"></panel>
     </scroller>

      <tabbar solt="bottom">
        <tabbar-item>
          <img slot="icon" src="./assets/icon_nav_button.png">
          <span slot="label">首页</span>
        </tabbar-item>
        <tabbar-item :selected="true">
          <img slot="icon" src="./assets/icon_nav_article.png">
          <span slot="label">热点</span>
        </tabbar-item>
        <tabbar-item>
          <img slot="icon" src="./assets/icon_nav_cell.png">
          <span slot="label">推荐</span>
        </tabbar-item>
        <tabbar-item>
          <img slot="icon" src="./assets/icon_nav_msg.png">
          <span slot="label">游戏</span>
        </tabbar-item>
      </tabbar>
    </view-box>
    <router-view></router-view>
  </div>
</template>

<script>
import {ViewBox,XHeader,Tabbar,TabbarItem,Tab, TabItem, Scroller as Sc, Swiper, Marquee, MarqueeItem,Panel} from  'vux'

var refreshKey =['A','B01','B02','B03','B04','B05','B06','B07','B08','B09','B010']

var key = 0;
var start = 0;
var end = start + 9;
var keyValue = 'A';
var initLoaded = false; //初始化是否加载完成
function getRefreshKey(){
  key++;
  if(key == refreshKey.length-1){
    key = 0
  }
  keyValue = refreshKey[key];
}
export default {
  name: 'app',
  components:{
    ViewBox,
    XHeader,
    Tabbar,
    TabbarItem,
    Tab,
    TabItem,
    Sc,
    Swiper,
    Marquee,
    MarqueeItem,
    Panel
  },
  created() {
    let arr = [1,2,3,4];
    arr.map((value,index)=>{
        return value + 2;
    })
    this.$jsonp('http://3g.163.com/touch/jsonp/sy/recommend/0-9.html').then(data => {
        //console.log(data)
      // 轮播图数据
         this.swiperList = data.focus.filter(item => {
           console.log(item.addata === null && item.picInfo[0])
           return item.addata === null && item.picInfo[0]

         }).map(item => {
            return {
              url: item.link,
              img: item.picInfo[0].url,
              title: item.title
            }
         });
      // 首屏新闻列表的数据
        this.datalist = data.list.filter(item => {
          return item.addata === null && item.picInfo[0]
        }).map(item => {
          return {
            src: item.picInfo[0].url,
            title: item.title,
            desc: item.digest,
            url:item.link
          }
        });
      initLoaded = true
      //滚动数据
        this.marqueeList = data.live.filter(item => {
          return item.addata === null && item.picInfo[0]
        }).map(item => {
          return {
            title: item.title,
          }
        })
    })
  },
  data(){
//      var dataList = [];
//      for(var i=0;i<10;i++){
//        dataList.push({
//          src: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
//          title: '标题一',
//          desc: '由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。',
//          url: '/component/cell'
//        })
//      }
      return {
        swiperList: [],
        swiperIndex: 0 ,// 设置当前第几张
        marqueeList:[],
        datalist: [],
        moreDataList: []
      }
  },
  methods: {
      refresh(){
         // console.log(1)
        //下拉数据加载
        getRefreshKey()
        this.$jsonp('http://3g.163.com/touch/jsonp/sy/recommend/0-9.html',{
          miss: '00',
          refresh: keyValue
        }).then(data => {
            console.log(data)
            //console.log(this.$refs.myRef)
          //刷新首页内容高度列表的数据
          this.datalist = data.list.filter(item => {
            return item.addata === null && item.picInfo[0]
          }).map(item => {
            return {
              src: item.picInfo[0].url,
              title: item.title,
              desc: item.digest,
              url:item.link
            }
          });
          this.$refs.myRef.finishPullToRefresh();
          //this.$vux.toast.show('hello')
          this.$vux.toast.text(`当前一共刷新了${this.datalist.length} 条数据`,'top')
        })
      },
      infinite(){
          //console.log(2)
        ///上拉加载更多
        if(!initLoaded){
          this.$refs.myRef.finishInfinite();
          return;
        }
        console.log(2);
        this.$jsonp(`http://3g.163.com/touch/jsonp/sy/recommend/${start}-${end}.html`,{
          miss: '00',
          refresh: keyValue
        }).then(data => {
            //上拉加载
          setTimeout(() =>{
            var dataList = data.moreDataList = data.list.filter(item => {
              return item.addata === null && item.picInfo[0]
            }).map(item => {
              return {
                src: item.picInfo[0].url,
                title: item.title,
                desc: item.digest,
                url:item.link
              }
            });
            this.moreDataList = this.moreDataList.concat(dataList)
            start +=10;
            end = start + 9;

          this.$refs.myRef.finishInfinite();
          },1000)
        });
      }
  }
}
</script>

<style lang="less">
  @import '~vux/src/styles/reset.less';// 样式重置表
  html,body{
    margin:0;
    width:100%;
    height:100%;
    overflow: hidden;
  }
  #app{
    height:100%;
  }
  #app .header{
    width:100%;
    position: absolute;
    top:0;
    left:0;
    z-index: 9;
  }
  .tab{
    width:1200px;
    margin-top: 46px;
  }
  #app .my-scroller{
    top:90px;
  }
  .my-marquee{
    margin:10px;
  }
  .loading-layer{
    padding-bottom: 90px;
  }
  .weui-media-box_appmsg .weui-media-box__hd{
    width:102px !important;
    height:60px;
  }
  .weui-media-box_appmsg .weui-media-box__hd img{
    width: 102px;
    height:60px;
  }
  .weui-media-box__bd {
    height:60px;
  }
</style>
