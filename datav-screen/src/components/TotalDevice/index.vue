<template>
  <div class="total-device">
    <div class="total-device-left">
      <div id="total-device-chart" />
    </div>
    <div class="total-device-right">
      <div class="title-wrapper">
        <div class="left">
          <div class="title">慕课外卖登录设备</div>
          <div class="sub-title">Distribution of Internet devices</div>
        </div>
        <div class="right">
          <div class="age">
            <count-to
              :startVal="startNum"
              :endVal="num"
              :duration="1000"
            />
            <span class="age-unit">台</span>
          </div>
        </div>
      </div>
      <div class="average-data-wrapper">
        <div class="average-data" v-for="(item, index) in refData" :key="index">
          <div class="average-data-value">
            <count-to
              :startVal="item.startValue"
              :endVal="item.value"
              :duration="1000"
            />
          </div>
          <div class="average-data-axis">
            <div class="point" :style="{background: item.color}" />
            <div class="text">{{item.name}}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Echarts from 'echarts'
  import { ref, watch } from 'vue'

  const color = ['rgb(176,207,120)', 'rgb(157,195,91)', 'rgb(131,167,72)']

  export default {
    name: 'totalDevice',
    props: {
      data: Object
    },
    setup(props) {
      let chart
      const refData = ref([])
      const num = ref(0)
      const startNum = ref(0)
      const updateChart = () => {
        function createOption() {
          return {
            series: [{
              name: '访问来源',
              type: 'pie',
              radius: '70%',
              selectedMode: 'multiple',
              selectedOffset: 10,
              clockwise: true,
              center: ['50%', '50%'],
              color,
              emphasis: {
                itemStyle: {
                  color: 'rgb(140,251,182)'
                }
              },
              data: refData.value,
              roseType: 'radius',
              label: { show: false }
            }]
          }
        }
        if (!chart) {
          chart = Echarts.init(document.getElementById('total-device-chart'))
        }
        chart.setOption(createOption())
      }
      watch(() => props.data, (newData) => {
        if (refData.value.length > 0) {
          refData.value = newData.devices.map((item, index) => ({
            startValue: refData.value[index].value,
            value: item.value,
            name: item.key,
            color: color[index]
          }))
        } else {
          refData.value = newData.devices.map((item, index) => ({
            startValue: 0,
            value: item.value,
            name: item.key,
            color: color[index]
          }))
        }
        startNum.value = num.value
        num.value = +newData.totalDevices
        updateChart()
      })
      return {
        refData,
        num,
        startNum
      }
    }
  }
</script>

<style lang="less" scoped>
  .total-device {
    display: flex;
    width: 100%;
    height: 100%;
    background: rgb(43, 44, 46);

    .total-device-left {
      width: 30%;
      height: 100%;

      #total-device-chart {
        width: 100%;
        height: 100%;
      }
    }

    .total-device-right {
      flex: 1;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      padding: 40px 10px 40px 0;

      .title-wrapper {
        display: flex;
        align-items: center;

        .left {
          .title {
            font-size: 32px;
          }

          .sub-title {
            font-size: 16px;
            margin-top: 10px;
          }
        }

        .right {
          flex: 1;
          margin-left: 40px;
          font-weight: bold;

          .age {
            font-size: 56px;
            font-family: DIN;

            .age-unit {
              font-size: 20px;
            }
          }
        }
      }

      .average-data-wrapper {
        display: flex;

        .average-data {
          flex: 1;
          display: flex;
          flex-direction: column;
          align-items: center;
          justify-content: center;
          font-size: 30px;
          font-weight: bolder;

          .average-data-value {
            width: 100%;
          }

          .average-data-axis {
            display: flex;
            align-items: center;
            width: 100%;
            margin-top: 5px;

            .point {
              width: 10px;
              height: 10px;
              border-radius: 50%;
            }

            .text {
              margin-left: 10px;
              font-size: 16px;
            }
          }
        }
      }
    }
  }
</style>
