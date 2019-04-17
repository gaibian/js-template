<template>
  <div class="dashboard-container">
    <menu-nav></menu-nav>
    <div class="btn" @click="handleCenterClick">设置地图层级</div>
    <div id="container"></div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import menuNav from './menuNav.vue'
export default {
  name: 'Dashboard',
  components:{
    menuNav
  },
  data() {
    return {
      positionP:0,
      positionQ:0,
      map:null
    }
  },
  created() {
    console.log('ceshi')
  },
  mounted() {
    this.createMap();
  },
  methods:{
    handleCenterClick() {
      var zoom = Math.floor(Math.random() * 7) + 11;
      // var lng = 121.138398 + Math.floor(Math.random() * 589828) / 1e6;
      // var lat = 30.972688 + Math.floor(Math.random() * 514923) / 1e6;
      // var lng = data.position.P;
      // var lat = data.position.Q; 
      this.map.setZoom(zoom)
      
    },
    createMap() {
      // this.map = new AMap.Map('container', {
      //     zoom:15,//级别
      //     resizeEnable: true,
      //     center: [121.57900597259, 29.885258965918],//中心点坐标
      //     viewMode:'3D'//使用3D视图
      // });
      this.map = new AMap.Map('container',{
        zoom:15
      })
      console.log(this.map)
      //使用高德地图插件
      this.map.plugin('AMap.Geolocation',() => {
        var geolocation = new AMap.Geolocation({
          enableHighAccuracy: true,
          timeout: 10000,
          buttonOffset: new AMap.Pixel(10,20),
          zoomToAccuracy: true,
          buttonPosition: 'RB'
        })
        geolocation.getCurrentPosition()

        var onComplete = (data) => {  //浏览器获取到正确的定位信息
          console.log(`获取到定位信息`)
          console.log(data);
          var zoom = Math.floor(Math.random() * 7) + 11;
          this.positionP = data.position.getLat();
          this.positionQ = data.position.getLng();
          data.position.getLng() //定位成功返回的经度
          data.position.getLat() //定位成功返回的纬度
      
          
          console.log(data.position.getLng())
          // this.map.setZoomAndCenter(zoom,[data.position.getLat(), data.position.getLng()])
        }
        var onError = (data) => {  //获取定位信息失败
          console.log(`失败${data}`)
          console.log(data)
        }
        AMap.event.addListener(geolocation, 'complete', onComplete)
        AMap.event.addListener(geolocation, 'error', onError)
      })
      
    },
    setCenterMap() {

    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
.dashboard-container {
  width:100%;
  height:100%;

}
#container{
  width:100%;
  height:100%;
}
.btn{
  position:fixed;
  top:200px;
  right:20px;
  background:red;
  font-size:16px;
  color:#fff;
  z-index:99;
  padding:12px 6px;
  cursor:pointer;
}
</style>
