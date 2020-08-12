<template>
  <div>
    <el-tabs type="border-card">
      <el-tab-pane label="广东地图">
        <div id="guangDongMap"></div>
      </el-tab-pane>
      <el-tab-pane label="深圳地图">
        <div id="shenZhenMap"></div>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
import echarts from "echarts";
import "echarts/map/js/province/guangdong.js";
import "./../assets/map/shenzhen.js";
import guangDongJson from "echarts/map/json/province/guangdong.json";
import shenZhenJson from "./../assets/map/shenzhen.json";
export default {
  data() {
    return {
      listArr: [], //城市json
      max: "", //最大value值
      min: "", // 最小value值
      listArr2: [], //区json
      max2: "", //最大value值
      min2: "" // 最小value值
    };
  },
  created() {
    this.getData();
    this.getData2();
  },
  mounted() {
    this.DrawMap();
    this.DrawMap2();
  },
  methods: {
    getData() {
      // 获取城市名称数据
      console.log("取到的广东省的json数据", guangDongJson);
      if (guangDongJson) {
        let arr = guangDongJson.features;
        // 循环取出 城市名称和value值
        for (var j = 0; j < arr.length; j++) {
          this.max = arr[0].id;
          this.min = arr[0].id;
          if (arr[j].id > this.max) {
            this.max = arr[j].id;
          }
          if (arr[j].id < this.min) {
            this.min = arr[j].id;
          }
          this.listArr.push({
            name: arr[j].properties.name,
            value: arr[j].id
          });
        }
      }
    },
    getData2() {
      // 获取城市名称数据
      console.log("深圳市json数据", shenZhenJson);
      if (shenZhenJson) {
        let arr = shenZhenJson.features;
        // 循环取出 城市名称和value值
        for (var j = 0; j < arr.length; j++) {
          this.max2 = arr[0].id;
          this.min2 = arr[0].id;
          if (arr[j].id > this.max2) {
            this.max2 = arr[j].id;
          }
          if (arr[j].id < this.min2) {
            this.min2 = arr[j].id;
          }
          this.listArr2.push({
            name: arr[j].properties.name,
            value: arr[j].id
          });
        }
      }
    },
    DrawMap() {
      let _this = this;
      let myMapChart = this.$echarts.init(
        document.getElementById("guangDongMap")
      );
      myMapChart.setOption({
        visualMap: {
          min: _this.min,
          max: _this.max,
          show: false,
          inRange: {
            color: [
              "lightskyblue",
              "yellow",
              "orangered",
              "#ffcccc",
              "#9999cc",
              "#66cc66",
              "#3366ff",
              "#00ff99",
              "red",
              "pink"
            ]
          }
        },
        series: [
          {
            type: "map",
            map: "广东",
            itemStyle: {
              normal: { label: { show: true } },
              emphasis: { label: { show: true } },
              emphasis: {
                areaColor: "#67C23A" //鼠标进入时的颜色
              }
            },
            data: _this.listArr
          }
        ]
      });
    },
    DrawMap2() {
      let _this = this;
      let myMapChart = this.$echarts.init(
        document.getElementById("shenZhenMap")
      );
      myMapChart.setOption({
        visualMap: {
          min: _this.min2,
          max: _this.max2,
          show: false,
          inRange: {
            color: ["lightskyblue", "yellow", "pink", "orangered", "red"]
          }
        },
        series: [
          {
            type: "map",
            map: "深圳",
            itemStyle: {
              normal: { label: { show: true } },
              emphasis: { label: { show: true } },
              emphasis: {
                areaColor: "#67C23A" //鼠标进入时的颜色
              }
            },
            data: _this.listArr2
          }
        ]
      });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#guangDongMap {
  height: 500px;
  width: 1200px;
}
#shenZhenMap {
  height: 500px;
  width: 1200px;
}
</style>
