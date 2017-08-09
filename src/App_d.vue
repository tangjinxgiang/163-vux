<template>
  <div id="app"> <!--作文大全-->
    <h2>作文大全</h2>
    <div class="classify"> <!--作文分类列表 -->
      请输入查询条件：<input class="input1" placeholder="1为年级，2为题材，3为字数，4为等级" v-model="txt_id" type="text"><input @click="seek" type="button" value="搜索">
    </div>

  </div>
</template>
<script type="text/ecmascript-6">
  import axios from 'axios';
    export default {
      data(){
        return{
          txt_id:'1',//根据输入的数字 规定类型
          name:'',//年级 ||题材 ||字数 ||等级 类型
          id:'' // 年级|| 题材 ||字数 ||等级 id
        }
      },
      methods:{
        seek: function(){
            axios.get('/zuowen/typeList', {
              params: {
                key: 'd7bed4021b582a13108e5c950d67472e',
                id: this.txt_id// 	1为年级，2为题材，3为字数，4为等级

              }
            })
              .then(function (response) {
                  if(response.data.error_code === 0){
                      //this.name = response.data.result
                    for(var i=0;i< response.data.result.length;i++){
                      var aLi = document.createElement('li')
                    }
                  }
              })
              .catch(function (error) {
                console.log(error);
              });
          }
      }
    }
</script>
<style>
.input1{width:300px;}
</style>
