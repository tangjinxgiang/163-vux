<template>
  <div id="app"> <!--作文大全-->
    <input type="text" id="q">
    <input type="button" value="按钮" @click="seek" id="btn">
    <p id="msg"></p>
    <hr>
    <div id="list">

    </div>
  </div>
</template>
<script type="text/ecmascript-6">
  import axios from 'axios';
  export default {
    data(){
      return{

      }
    },
    methods:{
      seek: function(){
        axios.get('/book/subjects', {
          params: {
            alt:'xd',
            callback: 'fn1'
          },
          timeout: 1
        })
        .then(function (response) {
           if(response.data.error_code === 0){

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
