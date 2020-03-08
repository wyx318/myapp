<template>
	<div class="hello">
		<!--    初始化echarts 需要个 有宽高的-->
		<div ref='mapbox' style="height: 400px;width: 600px;"></div>
	</div>
</template>

<script>
  import echarts from 'echarts'
  import 'echarts/map/js/china.js'
  import jsonp from 'jsonp'
  // const option = {
  //   xAxis: {
  //     type: 'category',
  //     data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
  //   },
  //   yAxis: {
  //     type: 'value'
  //   },
  //   series: [{
  //     data: [820, 932, 901, 934, 1290, 1330, 1320],
  //     type: 'line'
  //   }]
  // };
  //使用地图要先注册
  const option = {
    title: {
      text: "疫情地图",
      link: 'https://www.baidu.com',
      subtext: '十年专注'
    },
    series: [{
      name: '确诊人数',
      type: 'map',//告诉echarts去渲染一个地图
      map: 'china',//告诉echarts要去渲染中国地图
      label: {
        //控制对应地区的省份汉字
        show: true,
        color: '#333',//控制地区名字的颜色
        fontSize: '10'//控制字体大小
      },
      itemStyle: {
        // areaColor: " green" //控制地图的颜色
      },
      zoom: 1.2,//控制地图的大小，
      emphasis: {
        //控制鼠标滑过之后的 背景颜色和字体颜色
        label: {
          color: '#fff',
          fontSize: 12
        },
        itemStyle: {
          areaColor: "#66a9ec"
        }
      },
      data: [] //用来展示 后台给的数据
    }],
    visualMap: [{
      //状态
      type: 'piecewise',
      show: true,
      //默认五段
      // splitNumber: 4
      pieces: [
        //分段
        {min: 10000},
        {min: 1000, max: 9999},
        {min: 100, max: 999},
        {min: 10, max: 99},
        {min: 1, max: 9}
      ],
      // align: 'right' 控制方块的位置 默认是left
      // orient: 'horizontal'  默认竖直显示
      //left right 这些属性控制 分段的位置
      // showLable :false //显示方块 后面的数字
      inRange: {
        //控制方块的颜色等属性
        symbol: 'rect',
        color: ['#ffc0b1', '#9c0505']
      },
    }],
    tooltip: {
      trigger: 'item'
    }
  };
  export default {
    name: 'HelloWorld',
    mounted() {
      this.getDate()
      this.mychart = echarts.init(this.$refs.mapbox);
      this.mychart.setOption(option)
    },
    methods: {
      getDate() {
        // eslint-disable-next-line no-unused-vars
        jsonp('https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427&callback=__jp0', {}, (err, data) => {
          if (!err) {
            //代表请求数据成功
            console.log(data)
            let list = data.data.list.map(item => ({name: item.name, value: item.value}))
            option.series[0].data = list
            this.mychart.setOption(option)// 这行代码执行的前提是 Dom 渲染完成 只有dom渲染完成 才能够执行 echarts初始化
          }
        })
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	h3 {
		margin: 40px 0 0;
		}
	ul {
		list-style-type: none;
		padding: 0;
		}
	li {
		display: inline-block;
		margin: 0 10px;
		}
	a {
		color: #42b983;
		}
</style>
