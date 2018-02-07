<template>
  <div id="app">
    <div class="panel-body">
      <el-row>
         <el-col :span="8">
              <el-select  v-model="theme" placeholder="请选择产品"  @change="changeTheme">
                 <el-option v-for="item in themeList" :key="item.text" :label="item.text" :value="item.value"></el-option>
             </el-select>
           </el-col>
        </el-row>
    <el-row>
    <div class="chart-containter-box">
      <chart v-if='loading'  class="chart"  :option="option" ref="chart"  :resizable="true"   width="100%" :theme="theme" ></chart>
    </div>
    </el-row>
    </div>
    <router-view/>
  </div>
</template>
<script>
import Vue from 'vue'
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'
import ECharts from 'vue-echarts-v3/src/full.js'
import 'echarts/theme/macarons.js'
import 'echarts/theme/dark.js'
import 'echarts/theme/roma.js'
import 'echarts/theme/shine.js'
import 'echarts/theme/vintage.js'
import 'echarts/theme/infographic.js'
// require('echarts/theme/macarons')
Vue.use(ElementUI)
export default {
  name: 'App',
  components: {
    chart: ECharts
  },
  data: () => ({
    loading: true,
    theme: 'macarons',
    themeList: [{text: 'macarons', value: 'macarons'},
      {text: 'dark', value: 'dark'},
      {text: 'roma', value: 'roma'},
      {text: 'shine', value: 'shine'},
      {text: 'vintage', value: 'vintage'},
      {text: 'infographic', value: 'infographic'}],
    option: {
      title: {text: 'Ehart hello world'},
      grid: {
        show: true,
        top: 100,
        backgroundColor: '#ffd033cf'
      },
      tooltip: {
        trigger: 'axis',
        show: true,
        showDelay: 0,
        hideDelay: 50,
        transitionDuration: 0,
        backgroundColor: 'rgba(255,0,255,0.7)',
        borderColor: '#f50',
        borderRadius: 8,
        borderWidth: 2,
        padding: 10,
        position: function (p) {
          // 位置回调
          // console.log && console.log(p);
          return [p[0] + 10, p[1] - 10]
        },
        textStyle: {
          color: 'yellow',
          decoration: 'none',
          fontFamily: 'Verdana, sans-serif',
          fontSize: 15,
          fontStyle: 'italic',
          fontWeight: 'bold'
        },
        formatter: function (params, ticket, callback) {
          // tooltip 显示内容
          var res = 'Function formatter : <br/>' + params[0].name
          for (var i = 0, l = params.length; i < l; i++) {
            res += '<br/>' + params[i].seriesName + ' : ' + params[i].value
          }
          setTimeout(function () {
            // 仅为了模拟异步回调
            callback(ticket, res)
          }, 1000)
          return 'loading'
        }
      },
      // 工具栏
      toolbox: {
        show: true,
        feature: {
          mark: {show: true},
          dataView: {show: true, readOnly: false},
          magicType: {show: true, type: ['line', 'bar', 'stack', 'tiled']},
          restore: {show: true},
          saveAsImage: {show: true}
        }
      },
      // 是否显示拖拽
      calculable: true,
      xAxis: {
        // type有 value，category, time,log default:category
        type: 'category',
        name: 'X  轴',
        // 坐标间距
        nameGap: 25,
        // 坐标角度
        nameRotate: 30,
        triggerEvent: true,
        nameLocation: 'start',
        data: ['周一', '周二', '周三', '周四', '周五', '周六', '周日']
      },
      yAxis: [{
        type: 'value',
        name: 'Y  轴1',
        nameGap: 25,
        nameRotate: 30,
        triggerEvent: true,
        axisLabel: {
          rotate: 30,
          color: '#0e0e0ecf',
          formatter: '{value} k'
        }
      },
      {
        type: 'value',
        name: 'Y  轴2',
        nameGap: 25,
        nameRotate: 30,
        triggerEvent: true,
        axisLabel: {
          rotate: 30,
          color: '#0e0e0ecf',
          formatter: '{value} M'
        }
      }],
      series: [
        {
          name: '坐标轴触发1',
          type: 'bar',
          yAxisIndex: 0,
          data: [
            {value: 320, extra: 'Hello~'},
            332, 301, 334, 390, 330, 320
          ],
          // sybol:'circle', 'rect', 'roundRect', 'triangle', 'diamond', 'pin', 'arrow'
          markPoint: {symbol: 'triangle'}
        },
        {
          name: '坐标轴触发2',
          type: 'bar',
          markPoint: {symbol: 'diamond'},
          yAxisIndex: 1,
          data: [862, 1018, 964, 1026, 1679, 1600, 157]
        },
        {
          name: '数据项触发1',
          type: 'bar',
          tooltip: {// Series config.
            trigger: 'item',
            backgroundColor: 'black',
            position: [0, 0],
            formatter: 'Series formatter: <br/>{a}<br/>{b}:{c}'
          },
          stack: '数据项',
          yAxisIndex: 0,
          data: [
            120, 132,
            {
              value: 301,
              itemStyle: {normal: {color: 'red'}},
              tooltip: {
                // Data config.
                backgroundColor: 'blue',
                formatter: 'Data formatter: <br/>{a}<br/>{b}:{c}'
              }
            },
            134, 90,
            {
              value: 230,
              tooltip: {show: false}
            },
            210
          ]
        },
        {
          name: '数据项触发2',
          type: 'bar',
          tooltip: {
            show: false,
            trigger: 'item'
          },
          stack: '数据项',
          yAxisIndex: 1,
          data: [150, 232, 201, 154, 190, 330, 410]
        }
      ],
      legend: {
        padding: 50,
        data: ['坐标轴触发1', '坐标轴触发2', '数据项触发1', '数据项触发2']
      }
    }
  }),
  mounted () {
  },
  methods: {
    changeTheme () {
      this.loading = false
      // this.$refs.chart.theme = this.theme
      this.bar.title.text = 'Hello World'
      this.loading = true
    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.panel{
  padding: 10px;
  background-color: #fff;
  margin-bottom: 20px;
  border: 1px solid transparent;
  h3{
 border-bottom: 2px solid #2196f3;
 float: left;
 padding: 10px 10px;
 font-weight: 400;
  }
  .panel-heading{
  border-bottom: 1px solid #e9e9e9;
  margin: 0 15px;
  height: 50px;
 }
 .panel-body{
  margin: 20px 20px;
  overflow: hidden;
}
}
 .chart-containter-box {
    width: 100%;
    height: 600px;
    padding: 10px;
    background: white;
    border: 1px solid #d1dbe5;
    box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.12), 0 0 6px 0 rgba(0, 0, 0, 0.04);
    .chart{
    width: 100%;
    height: 400px;
    }
  }
</style>
