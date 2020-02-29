<template>
  <div class="hello">
    <!-- 初始化echarts需要个有宽高的盒子 -->
    <div ref="mapbox" style="height:500px;width:600px; margin:0 auto"></div>
  </div>
</template>

<script>
import echarts from 'echarts'
import 'echarts/map/js/china.js'
import jsonp from 'jsonp'
// const option = {
//     xAxis: {
//         type: 'category',
//         data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
//     },
//     yAxis: {
//         type: 'value'
//     },
//     series: [{
//         data: [820, 932, 901, 934, 1290, 1330, 1320],
//         type: 'line'
//     }]
// };

  // 使用地铁需要先注册地图
  const option ={
    title:{
      text:'小啊',
      link:'https//hao123.com',
      subtext:'你猜猜',
      sublink: 'https://baidu.com'
    },
    series:[{
      name:'确诊人数',
      type:'map',//告诉echarts 要去渲染的是一个地铁
      map:'china',//告诉echarts 要去渲染中国地铁
      label:{
        //控制对应地区的汉字
        show:true,
        color:'#333',
        fontSize:7
      },
      itemStyle:{
        //地图板块的颜色和边框
        areaColor:'#eee',
        // borderColor:'blue'
      },
      roam:true,//控制地图放大缩小
      zoom:1.3,//控制地图的放大和缩小
      emphasis:{
        //控制鼠标滑过之后的背景色和字体颜色
        label:{
          color:'#fff',
          fontSize:8
        },
        itemStyle:{
          areaColor:'#83b5e7'
        },
      
      },
      data:[]//用来展示后台数据的{name:xx,value:xx}
    }],

    //图标和字体的样式
    visualMap:[{
       type : 'piecewise',
       show:true,
      //  splitNumber:4
      pieces:[
        //分段
        {min:10000},
        {min:1000,max:9999},
        {min:100,max:999},
        {min:10,max:99},
        {min:1,max:9}
      ],
      // align:'right'//默认left
      // orient:'horizontal'//默认竖直
      //left right 这些属性控制分段所在的位置
      // showLabel:false
      //textStyle:{}
      inRange:{
        symbol:'rect',
        color:['#ffc0b1','#9c0505']
      },
       itemWidth:20,
       itemHeight:10
    }],
    //控制鼠标滑过显示文字
    tooltip:{
      trigger: 'item'
    },
    //右边的下载功能
     toolbox: {
          show: true,
          orient: 'vertical',
          left: 'right',
          top: 'center',
          feature: {
              dataView: {readOnly: false},
              restore: {},
              saveAsImage: {}
          }
      },
  }
export default {
  name: 'HelloWorld',
  mounted(){
    this.getDate();//为什么不在created
    // 先初始化
    // this.$refs获取元素
   this.mychart = echarts.init(this.$refs.mapbox)
   this.mychart.setOption(option)
  },
  methods:{
    getDate(){
       jsonp('https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427',{},(err,data)=>{
         if(!err){
           //代表请求成功
           console.log(data)
           let list= data.data.list.map(item=>({name:item.name,value:item.value}))
           option.series[0].data=list;
           this.mychart.setOption(option)//这行代码能执行的前提是 DOM渲染才能够执行 echarts初始化
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
