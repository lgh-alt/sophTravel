<template>
  <div class="wrapper">
    <div class="amap-wrapper" >
      <el-amap 
        ref="map"
        class="amap-box" 
        vid="amap"
        :center="center"
        :zoom="zoom"
        :resizeEnable="true"
        :events="mapEvents">
          <div
            v-for="(marker,index) in markers"
            :key="index">
            <el-amap-marker
              ref="marker"
              :position="marker.position"
              :icon="dwIcon"
              :events="marker.events">
              
              </el-amap-marker>
          </div>
      </el-amap>
    </div>
  </div>
</template>
<script>
import deflultData from '@/assets/js/marker2.js'
import dwicon from '@/assets/img/dw.png'
export default {
  name:"ScenicFlow3",
  data () {
    return {
      center:[120.76,30.77],
      zoom:10,
      markers:null,
      dwIcon:dwicon,
      mapEvents:{
        complete: () => {
          this.$refs.map.$$getInstance().setFitView()
          let map = this.$refs.map.$$getInstance();
          console.log(map)
        },
        init(o) {
          o.setMapStyle('amap://styles/blue')
        },
      },
    }
  },
  mounted () {
    this.initData();
  },
  methods:{
    initData () {
      let markers = []
      let self = this
      deflultData.forEach(item => {
        markers.push ({
          area:item.area,
          position:item.position,
          events:{
            click () {
              console.log(item.position)
              self.$refs.map.$$getInstance().setZoom(16)
              self.$refs.map.$$getInstance().setCenter(item.position)
            }
          }
        })
      });
      this.markers = markers;
      console.log(this.markers)
    }
  }
}
</script>
<style lang="less" scoped>
  .amap-wrapper {
    height: 520px;
  }
</style>