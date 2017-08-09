<template>
  <div id="app"> <!--身份证信息-->
    <input type="text" id="txt" v-model="identity">
    <input @click="btn" type="button" id="btn" value="查询">
    <div>
      姓名：<p id="p1">{{area}}</p><br>
      性别：<p id="p2">{{sex}}</p><br>
      出生日期：<p id="p3">{{birthday}}</p>
    </div>
  </div>

</template>
<script type="text/ecmascript-6">
      import axios from 'axios';
      var oTxt = document.getElementById('txt');
      export default {
        data(){
          return{
            identity: '431229199708210034',
            area:'',
            sex:'',
            birthday:''
          }
        },
        methods: {
          btn: function(){
            axios.get('/idcard/index',{
                params: {
                  key: '720e3b38f1da0d064b76b871eb431981',
                  cardno:this.identity,
                  dtype:'json'
                }
            })
            .then((response) => {
      console.log(response)
                if(response.data.error_code === 0){

                  this.area = response.data.result.area;

                  this.sex = response.data.result.sex;
                  this.birthday = response.data.result.birthday;

                } else{
                  alert(response.data.reason)
                }
            })
            .catch(function (error) {
                console.log(error);
            });
          }
        }
      }

</script>
<style lang="less">
  @import '~vux/src/styles/reset.less';// 样式重置表
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
