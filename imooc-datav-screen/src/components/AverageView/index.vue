<template>
  <div class="average-view">
    <div class="left">
      <div class="title">实时订单趋势图</div>
      <div class="sub-title">Number Of Real-time Orders</div>
      <div class="total">
        <count-to
          :start-val="startVal"
          :end-val="endVal"
          :duration="1000"
          separator=","
          autoplay
        />
      </div>
      <div class="tiny-title">周同比增长率</div>
      <div class="percent-text">
        <span class="percent-text-1">
          <count-to
            :start-val="startPercent"
            :end-val="percent"
            :duration="1000"
            :decimals="2"
            suffix="%"
          />
        </span>
      </div>
    </div>
    <div class="right">
      <div id="average-view-chart" />
    </div>
  </div>
</template>

<script>
  import Echarts from 'echarts'
  import { watch } from 'vue'

  const kpi = 999999

  export default {
    name: 'totalUser',
    props: {
      data: Object
    },
    setup(props) {
      watch(() => props.data, () => {
        function createOption() {
          const option = {
            xAxis: {
              type: 'category',
              boundaryGap: false,
              data: props.data.date,
              axisLine: {
                lineStyle: {
                  color: 'rgba(200, 200, 200)'
                }
              },
              axisLabel: {
                fontSize: 16
              }
            },
            yAxis: {
              type: 'value',
              splitLine: {
                lineStyle: {
                  color: 'rgb(50, 50, 50)'
                }
              },
              axisLine: {
                lineStyle: {
                  color: 'rgba(200, 200, 200)'
                }
              },
              axisLabel: {
                fontSize: 16
              }
            },
            dataZoom: [{
              type: 'inside',
              start: 0,
              end: 100
            }, {
              start: 0,
              end: 100,
              handleIcon: 'M 0.0525 0.5656 L 0.0525 0 L -0.0583 0 L -0.0583 0.5656 L -0.2449 0.5656 L -0.2449 1.4344 L -0.0525 1.4344 L -0.0525 2 L 0.0525 2 L 0.0525 1.4344 L 0.2449 1.4344 L 0.2449 0.5656 L 0.0525 0.5656 Z M 0.1399 1.1953 L -0.1458 1.1953 L -0.1458 1.1137 L 0.1399 1.1137 L 0.1399 1.1953 Z M 0.1399 0.8863 L -0.1458 0.8863 L -0.1458 0.8047 L 0.1399 0.8047 L 0.1399 0.8863 Z',
              handleSize: '100%',
              handleStyle: {
                color: '#a7b7cc'
              },
              textStyle: {
                color: 'rgb(200, 200, 200)'
              },
              fillerColor: 'rgba(120,126,134,.3)',
              dataBackground: {
                lineStyle: {
                  color: 'grey'
                },
                areaStyle: {
                  color: 'gray'
                }
              },
              borderColor: 'rgb(200, 200, 200)'
            }],
            series: [
              {
                name: '模拟数据',
                type: 'line',
                smooth: true,
                symbol: 'none',
                sampling: 'average',
                itemStyle: {
                  color: new Echarts.graphic.LinearGradient(0, 0, 0, 1,
                    [
                      {
                        offset: 0,
                        color: '#C2C90B'
                      },
                      {
                        offset: 0.5,
                        color: '#A1DC14'
                      },
                      {
                        offset: 1,
                        color: 'rgb(188, 222, 129)'
                      }
                    ])
                },
                data: props.data.data
              }
            ],
            grid: {
              top: 40,
              bottom: 80,
              right: 40,
              left: 80
            }
          }
          return option
        }
        const chart = Echarts.init(document.getElementById('average-view-chart'))
        chart.setOption(createOption())
      })
    },
    data() {
      return {
        startVal: 0,
        endVal: 23242,
        startPercent: 0,
        percent: 23242 / kpi * 100
      }
    }
  }
</script>

<style lang="less" scoped>
  .average-view {
    display: flex;
    width: 100%;
    height: 100%;

    .left {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: flex-start;
      flex: 0 0 320px;
      width: 320px;
      padding-left: 40px;
      box-sizing: border-box;

      .title {
        font-size: 32px;
      }

      .sub-title {
        font-size: 16px;
        letter-spacing: 1px;
        margin-top: 10px;
      }

      .total {
        font-family: DIN;
        font-size: 68px;
        font-weight: bolder;
        letter-spacing: 2px;
        padding: 10px 0;
      }

      .tiny-title {
        font-size: 18px;
        color: #fff;
        line-height: 36px;
      }

      .percent-text {
        font-size: 28px;
        font-family: DIN;
        letter-spacing: 2px;

        .percent-text-1 {
          color: rgb(197, 251, 121);
          font-weight: bolder;
        }
      }
    }

    .right {
      flex: 1;

      #average-view-chart {
        width: 100%;
        height: 100%;
      }
    }
  }
</style>
