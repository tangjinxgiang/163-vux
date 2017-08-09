<template>
  <div id="app">  <!--历史天气-->
    <div class="province">
      <div>
        省份名称：<input v-model="txt_province" type="text"><input type="button" @click="province" value="查询省份id">
        <p class="p1">省份id:{{province_ID}}</p>
      </div>
      <div>
        省份id：<input v-model="province_id" type="text"><br><br>
        城市名称：<input type="text" v-model="cityName"><input @click="cityBtn" type="button" value="查询城市id">
        <p class="p1">{{cityId}}</p>
      </div>
    </div>
    <div class="weather">
      城市ID:<input  type="text" class="q" v-model="cityId"><br>
      日期： <input class="q" v-model="txt_date" placeholder="格式:2017-07-15，日期不能大于等于今日日期" type="text">
      <input type="button" value="查询" id="btn" @click="btn">
      <p id="msg"></p>
      <hr>
      <div id="list">
        <h2>历史天气查询：</h2>
        <ul>
          <li>城市地区名称:&nbsp&nbsp&nbsp{{name}}</li>
          <li>天气日期:&nbsp&nbsp&nbsp{{date}}</li>
          <li>白天天气:&nbsp&nbsp&nbsp{{day_weather}}</li>
          <li>夜间天气:&nbsp&nbsp&nbsp{{night_weather}}</li>
          <li>白天最高温度:&nbsp&nbsp&nbsp{{day_temp}}</li>
          <li>夜间最低温度:&nbsp&nbsp&nbsp{{night_temp}}</li>
          <li>白天风向:&nbsp&nbsp&nbsp{{day_wind}}</li>
          <li>白天分力:&nbsp&nbsp&nbsp{{day_wind_comp}}</li>
          <li>夜间风向:&nbsp&nbsp&nbsp{{night_wind}}</li>
          <li>夜间风力:&nbsp&nbsp&nbsp{{night_wind_comp}}</li>
        </ul>
      </div>
    </div>

  </div>
</template>

<script>
  import axios from 'axios';

  export default {
    data(){
        return{
          city:'',//查询城市天气输入的城市的Id
          txt_date:'',//查询天气的时间
          name:'',
          date:'',
          day_weather:'',
          night_weather:'',
          day_temp:'',
          night_temp:'',
          day_wind:'',
          day_wind_comp:'',
          night_wind:'',
          night_wind_comp:'',

          txt_province:'',//输入的省份名称
          province_ID:'',//获取的省份id
          province_id:'',//输入的省份id
          cityName:'',//输入的城市名称
          cityId:''//获取的城市id
        }
    },

    methods: {
        // 天气查询
      btn: function(){
        axios.get('/historyWeather/weather?',{
          params: {
            key: 'a152d2e433a00af1a4220d8ba826d8ef',
            city_id: this.cityId,
            weather_date: this.txt_date
          }
        })
          .then((response) => {
            console.log(response)
              if(response.data.error_code === 0){
                this.name =  response.data.result.city_name;
                this.date =  response.data.result.weather_date;
                this.day_weather =  response.data.result.day_weather;
                this.night_weather =  response.data.result.night_weather;
                this.day_temp =  response.data.result.day_temp;
                this.night_temp =  response.data.result.night_temp;
                this.day_wind =  response.data.result.day_wind;
                this.day_wind_comp =  response.data.result.day_wind_comp;
                this.night_wind =  response.data.result.night_wind;
                this.night_wind_comp =  response.data.result.night_wind_comp;

              } else{
                  alert('查询不到')
              }

          })
          .catch(function (error) {
            alert(error)
          });
      },
      // 省份ID
      province: function(){
        axios.get('/historyWeather/province?key=a152d2e433a00af1a4220d8ba826d8ef')
          .then((response) => {
            for(var i=0;i<response.data.result.length;i++){//response.data.result是数组循环每一项
              //如果输入框输入的省份名字和数据中的一样就显示当前省份ID
              //数据中的第i项符合条件进去 把当前i项的id赋值出来
              if(this.txt_province == response.data.result[i].province){
                  this.province_ID = response.data.result[i].id;
              }
            }
          })
          .catch(function (error) {
            console.log(error);
          });
      },
      //查询城市ID
      cityBtn: function(){
        axios.get('/historyWeather/citys', {
          params: {
            key: 'a152d2e433a00af1a4220d8ba826d8ef',
            province_id: this.province_id
          }
        })
          .then((response) => {
            for(var i=0;i<response.data.result.length;i++){
                //循环response.data.result 判断第i个里面的城市名称与输入的是否一样
               if(this.cityName == response.data.result[i].city_name){
                  this.cityId = response.data.result[i].id;//如果一样就把当前第i个里面的城市id获取
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
  .weather{
    width:480px;
    height:580px;
    border:1px solid red;
    padding:10px;
    float:left;
  }
  .province{
    width:350px;
    height:600px;
    border:1px solid red;
    float:left;
    margin-right:10px;
  }
  .p1{height:30px;border:1px solid red;text-align: left;line-height:30px;font-weight:bold;}
  .q {width: 400px; height: 30px;margin-bottom: 5px; padding: 5px; border:1px solid #f90; font-size: 16px;}
  dl {border-bottom: 1px dotted #000;}
  dt {font-weight: bold;}
  li{list-style:none;border-bottom:1px solid #666;width:200px;margin-top:5px;padding:5px;}
</style>
