<!--地图-->
<template>
    <div style="width: 100%;height: 100%;">
        <div ref="chart" class="echarts"></div>
    </div>
</template>
<script>
    import echarts from 'echarts'
    // import 'echarts-gl'
    const json = require("./LHMap");
    export default {
        props:{
            address_info:{
                type:Array,
                default:()=>{
                    return[]
                }
            },
            town_name:{
                type:String,
                default:"临海市"
            }
        },
        data(){
            return{
                chart: null,
                mapInfo:json,
                mapcolor:'#85a5ff'
            }
        },
        mounted() {
            // 基于准备好的dom，初始化echarts实例
            this.chart = echarts.init(this.$refs.chart);
            var that = this;
            this.chart.on('click', function (params) {
                if(params.componentType === "geo")
                {
                    let city = params.name;
                    if(that.town_name == city)
                        return;
                    for(let i in that.address_info)
                    {
                        if(that.address_info[i].name == city)
                        {
                            that.getAreaInfo(that.address_info[i].abbr, that.address_info[i].indexCode, params.name)
                            break;
                        }
                    }
                }
                else if(params.componentType === "series")
                {
                    var city = params.data.py;
                    if(that.mapInfo.hasOwnProperty(city))
                    {
                        // that.init(city)
                        that.getAreaInfo(city, params.data.indexCode, params.name)
                    }
                    else if(typeof params.data.value == 'object')
                    {
                        that.$router.push({
                            name:"Map_conmand",
                            params:{
                                name:params.data.name,

                            }
                        })
                    }
                }
            });
            //that.init("LinHai");
            // 浏览器自适应
            window.addEventListener('resize', (event)=> {
                echarts.init(option);
                // initCharts();
                this.chart.resize();
            })
        },
        methods:{
            map_resize()
            {
                this.chart.resize();
            },
            init(initData="LinHai"){
                // 指定图表的配置项和数据
                // var mapData = [
                //
                //     {'latitude':28.911139, 'longitude':121.201716, 'name':'大田街道新希望幼儿园', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.908932, 'longitude':121.214184, 'name':'临海市东塍镇中学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.908612, 'longitude':121.211643, 'name':'临海市东塍镇中心校', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.928965, 'longitude':121.279441, 'name':'临海市桐峙中学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.929863, 'longitude':121.280798, 'name':'临海市汇溪镇中心校', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.854852, 'longitude':121.466618, 'name':'临海市东塍镇第二小学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.964944, 'longitude':121.222291, 'name':'大田初级中学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.931469, 'longitude':121.258369, 'name':'临海江南中心小学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.923046, 'longitude':121.20289 , 'name':'临海汛桥镇中心校', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.826816, 'longitude':121.135505, 'name':'临海沿江镇中心校', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.797815, 'longitude':121.182479, 'name':'西岑小学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.74927, 'longitude':121.247081, 'name':'临海市尤溪镇中学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.738583, 'longitude':121.309496, 'name':'临海市灵江中学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.747137, 'longitude':121.111868, 'name':'临海市杜桥中学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.834603, 'longitude':121.133521, 'name':'临海市学海中学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.777519, 'longitude':121.508212, 'name':'上盘中学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.766684, 'longitude':121.495266, 'name':'桃渚中学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.760501, 'longitude':121.604628, 'name':'杜桥第二小学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.833992, 'longitude':121.599904, 'name':'临海市上盘镇中心校', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.768127, 'longitude':121.519365, 'name':'临海市头门港新区幼儿园', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.751542, 'longitude':121.599576, 'name':'临海杜桥市场小学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.731249, 'longitude':121.645981, 'name':'临海杜桥镇大汾巧克力幼儿园', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.765745, 'longitude':121.561679, 'name':'成职教中心', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.747014, 'longitude':121.490158, 'name':'大洋中心校', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.890153, 'longitude':121.199139, 'name':'回浦实验中学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.89355, 'longitude':121.195703, 'name':'回浦中学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.886792, 'longitude':121.164461, 'name':'台州学院临海校区', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.885385, 'longitude':121.164501, 'name':'涌泉镇中心校', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.88326, 'longitude':121.173077, 'name':'涌泉中学', 'value':100, 'color':'#ff6521'},
                // ]
                // var disAline = [
                //     {'latitude':28.865639, 'longitude':121.161312, 'name':'高级职业中学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.860924, 'longitude':120.997773 , 'name':'城西中学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.905503, 'longitude':120.90579 , 'name':'双港小学', 'value':100, 'color':'#ff6521'},
                //     {'latitude':28.960066, 'longitude':120.959857, 'name':'括苍镇中心幼儿园', 'value':100, 'color':'#ff6521'},
                // ];
                this.chart.showLoading();
                var result = this.mapInfo[initData]["mapInfo"];
                var that = this
                echarts.registerMap(initData, result); //加载地图数据
                this.chart.hideLoading();
                let option = null;
                this.chart.setOption(option = {
                    tooltip: {
                        trigger: 'item',
                        backgroundColor : 'transparent',
                        // formatter: function (params) {
                        //     if(typeof(params.data.value)== "object")
                        //         return '存在异常学校'+params.data.value[2]+"个";

                        //     // return params.name+"的摄像头个数："+params.value[2];
                        // }
                            formatter: function (params) {
                                if(params.color){
                                    that.mapcolor = params.color
                                }
                                if(typeof(params.data.value)== "object")
                                {
                                    // return params.data.name+'检测到异常数:'+params.data.value[2]+"个";
                                    var tipHtml = '';
                                    tipHtml = '<div style="display: inline-block;border-radius:3px;background:rgba(255,255,255,0.85);padding:15px;box-shadow:0 0 10px #666">'+
                                                '<div style="font-size:18px;line-height:20px;padding-bottom: 10px;color:#333">' + params.data.name + '</div>'+
                                                '<p style="font-size: 15px;color: #666;line-height: 25px;">异常学校数：<span style="color: red;font-size:16px">' + params.data.value2[0] + '</span></p>'+
                                                '<p style="font-size: 15px;color: #666;line-height: 25px;">后厨人数：<span style="font-size: 16px;color:#555">' + params.data.value2[1] + '</span></p>'+
                                                '<p style="font-size: 15px;color: #666;line-height: 25px;">食品安全员人数：<span style="font-size: 16px;color:#555">' + params.data.value2[2] + '</span></p>'+
                                                '<p style="font-size: 15px;color: #666;line-height: 25px;">超市人数：<span style="font-size: 16px;color:#555;">' + params.data.value2[3] + '</span></p>'+
                                                '<p style="font-size: 15px;color: #666;line-height: 25px;">健康证数量：<span style="font-size: 16px;color:#555">' + params.data.value2[4] + '</span></p>'+
                                                '</div>'
                                    return tipHtml;
                                }
                            }
                    },
                    legend: {
                        orient: 'vertical',
                        left: 'left',
                        data:["正常","异常"],
                        textStyle:{//图例文字的样式
                            color:'#ffffff',
                        },
                        icon:"pin",
                        bottom:"0",

                    },
                    toolbox: {
                        show: false, //此字段表示是否显示或启用
                        //orient: 'vertical', //工具栏
                        left: 'left',
                        top: 'top',
                        feature: {
                            dataView: {
                                readOnly: false
                            },
                            restore: {},
                            saveAsImage: {}
                        }
                    },
                    visualMap: {
                        type: 'continuous',
                        text: ['', ''],
                        showLabel: false,
                        seriesIndex: [0],
                        min: 0,
                        max: 7,
                        inRange: {// 地图色块列表
                            
                            color: ['#ff7875', '#ffc069', '#fff566', '#95de64', '#5cdbd3', '#69c0ff','#b37feb',]
                            // color: ['rgba(255, 77, 79,0.75)', 'rgba(255, 192, 105,0.75)', 'rgba(255, 245, 102,0.75)', 'rgba(149,222,100,0.75)', 'rgba(92,219,211,0.75)', 'rgba(105,192,255,0.75)','rgba(179,127,235,0.75)',]
                        },
                        textStyle: {
                            color: '#000'
                        },
                        bottom: 30,
                        left: 'left',
                        show :false
                    },
                    geo: {
                        map:initData,
                        zoom: 1.2,   //地图初始大小，这里是关键，一定要放在 series中  因为geo在series中会加载，所以zoom放在此处
                        itemStyle:{
                            areaColor:'transparent'
                        }
                        // show: true,
                        // roam: true,
                    },
                    series: [
                        {
                            name: initData,
                            type: 'map',
                            // coordinateSystem: 'geo',
                            map: initData,
                            zoom: 1.2,
                            showLegendSymbol: false,
                            data:this.address_info.map((item, index)=>{
                                return{
                                    name:item.name,
                                    value:[index%7],
                                    value2: [
                                        item.stranger_num,//异常人数
                                        item.chef_people,//后厨人数
                                        item.food_people,//食品安全员人数
                                        item.damp_people,//超市人数
                                        item.health,//健康证数量
                                    ],
                                    py:item.abbr,
                                    indexCode:item.indexCode,
                                    label:{
                                        normal: {
                                            show: true,
                                            color:'#f2f2f2',
                                            textStyle:{
                                                fontSize:11
                                            }
                                        },
                                        emphasis: {
                                            show:true,
                                            textStyle: {
                                                color: '#fff',
                                                fontSize:11
                                            }
                                        },
                                    },
                                    itemStyle: {
                                        normal: {
                                            borderColor: '#ffffff', //区域边框颜色
                                            borderWidth:3, //区域边框宽度
                                            areaColor: '#2648aa', //区域填充颜色
                                            color: {
                                                type: 'linear',
                                                x: 0,
                                                y: 0,
                                                x2: 0,
                                                y2: 1,
                                                colorStops: [{
                                                    offset: 0, color: '#096dd9' // 0% 处的颜色
                                                }, {
                                                    offset: 1, color: '#003a8c' // 100% 处的颜色
                                                }],
                                                global: false // 缺省为 false
                                            },
                                        },
                                        emphasis: {
                                            areaColor: 'rgba(24, 144, 255,0.7)',
                                            borderWidth: 0,
                                        }
                                    }
                                }
                            })
                        },
                        {
                            name: initData,
                            type: 'scatter',
                            coordinateSystem: 'geo',
                            data:this.address_info.map((item)=>{
                                return{
                                    name:item.name,
                                    value:item.abbr,
                                    py:item.abbr,
                                    indexCode:item.indexCode,
                                    itemStyle: {
                                        normal: {
                                            borderColor: '#ffffff', //区域边框颜色
                                            areaColor: '#253752', //区域填充颜色
                                            color: {
                                                type: 'linear',
                                                x: 0,
                                                y: 0,
                                                x2: 0,
                                                y2: 1,
                                                colorStops: [{
                                                    offset: 0, color: '#096dd9' // 0% 处的颜色
                                                }, {
                                                    offset: 1, color: '#002766' // 100% 处的颜色
                                                }],
                                                global: false // 缺省为 false
                                            }
                                        },
                                        emphasis: {
                                            areaColor: '#0a2dae',//悬浮区背景
                                        }
                                    },
                                }
                            })
                        },
                        {
                            name:"正常",
                            type: 'scatter',
                            coordinateSystem: 'geo',
                            showEffectOn: 'render',
                            showSymbol: true,
                            symbolSize:30,
                            symbol:"pin",
                            color:"#4caf50",
                            data: this.address_info.map(function (itemOpt) { //散点生成
                                if(itemOpt.abbr == 'lhs' || itemOpt.abbr == 'tmg')
                                    return;
                                if(itemOpt.is_normal != 1)
                                    return;
                                return {
                                    name: itemOpt.name,
                                    value: [
                                        itemOpt.longitude,
                                        itemOpt.latitude,
                                        itemOpt.stranger_num,
                                    ],
                                    py:itemOpt.abbr,
                                    indexCode:itemOpt.indexCode,
                                    itemStyle: {
                                        normal: {
                                            color: '#4caf50'
                                        }
                                    }
                                };
                            })
                        },
                        {
                            name:"异常",
                            type: 'scatter',
                            coordinateSystem: 'geo',
                            showEffectOn: 'render',
                            showSymbol: true,
                            symbolSize:30,
                            symbol:"pin",
                            color:"#d9001b",
                            data: this.address_info.map(function (itemOpt) { //散点生成
                                if(itemOpt.abbr == 'lhs' || itemOpt.abbr == 'tmg')
                                    return;
                                if(itemOpt.is_normal == 1)
                                    return;
                                return {
                                    name: itemOpt.name,
                                    value: [
                                        itemOpt.longitude,
                                        itemOpt.latitude,
                                        itemOpt.stranger_num
                                    ],
                                    py:itemOpt.abbr,
                                    indexCode:itemOpt.indexCode,
                                    itemStyle: {
                                        normal: {
                                            color: '#d9001b'
                                        }
                                    }
                                };
                            })
                        }
                    ],
                    grid:{
                        width:'100%',
                        height:'100%'
                    }
                },true);
            },
            changeCityMap(city, data)
            {
                this.chart.showLoading();
                var result = this.mapInfo[city]["mapInfo"];
                var mcolor = this.mapcolor
                echarts.registerMap(city, result); //加载地图数据
                this.chart.hideLoading();
                let option = null;
                this.chart.setOption(option = {
                    tooltip: {
                        trigger: 'item',
                        backgroundColor : 'transparent',
                        formatter: function (params) {
                            if(typeof(params.data.value)== "object")
                            {
                                // return params.data.name+'检测到异常数:'+params.data.value[2]+"个";
                                var tipHtml = '';
                                tipHtml = '<div style="display: inline-block;border-radius:3px;background:rgba(255,255,255,0.85);padding:15px;box-shadow:0 0 10px #666">'+
                                            '<div style="font-size:18px;line-height:20px;padding-bottom: 10px;color:#333">' + params.data.name + '</div>'+
                                            '<p style="font-size: 15px;color: #666;line-height: 25px;">异常人数：<span style="color: red;font-size:16px">' + params.data.value[2] + '</span></p>'+
                                            '<p style="font-size: 15px;color: #666;line-height: 25px;">后厨人数：<span style="font-size: 16px;color:#555">' + params.data.value[3] + '</span></p>'+
                                            '<p style="font-size: 15px;color: #666;line-height: 25px;">食品安全员人数：<span style="font-size: 16px;color:#555">' + params.data.value[4] + '</span></p>'+
                                            '<p style="font-size: 15px;color: #666;line-height: 25px;">超市人数：<span style="font-size: 16px;color:#555;">' + params.data.value[5] + '</span></p>'+
                                            '<p style="font-size: 15px;color: #666;line-height: 25px;">健康证数量：<span style="font-size: 16px;color:#555">' + params.data.value[6] + '</span></p>'+
                                            '</div>'
                                return tipHtml;
                            }

                            // return params.name+"的摄像头个数："+params.value[2];
                        }
                    },
                    legend: {
                        orient: 'vertical',
                        left: 'left',
                        data:["正常","异常"],
                        textStyle:{//图例文字的样式
                            color:'#ffffff',
                        },
                        icon:"pin",
                        bottom:"20",

                    },
                    toolbox: {
                        show: false, //此字段表示是否显示或启用
                        //orient: 'vertical', //工具栏
                        left: 'left',
                        top: 'top',
                        feature: {
                            dataView: {
                                readOnly: false
                            },
                            restore: {},
                            saveAsImage: {}
                        }
                    },
                    geo: {
                        map:city,
                        zoom: 1.2,   //地图初始大小，这里是关键，一定要放在 series中  因为geo在series中会加载，所以zoom放在此处
                        //roam: true,
                        label: {
                            normal: {
                                show: true,
                                color:'white'
                            },
                            emphasis: {
                                show: true,
                                color:'white'
                            },

                        },
                        itemStyle: {
                            normal: {
                                areaColor: mcolor,
                            //         type: 'linear',
                            //         x: 0,
                            //         y: 0,
                            //         x2: 0,
                            //         y2: 1,
                            //         colorStops: [{
                            //             offset: 0, color: '#3a7fd5' // 0% 处的颜色
                            //         }, {
                            //             offset: 1, color: '#002766' // 100% 处的颜色
                            //         }],
                            //         global: false // 缺省为 false
                            //     },
                            //     borderColor: '#0a53e9',//线
                            //     shadowColor: '#092f8f',//外发光
                            //     shadowBlur: 20
                            },
                            emphasis: {
                                areaColor: mcolor
                                //     {
                                //     type: 'linear',
                                //     x: 0,
                                //     y: 0,
                                //     x2: 0,
                                //     y2: 1,
                                //     colorStops: [{
                                //         offset: 0, color: '#002766' // 0% 处的颜色
                                //     }, {
                                //         offset: 1, color: '#3a7fd5' // 100% 处的颜色
                                //     }],
                                //     global: false // 缺省为 false
                                // },//悬浮区背景
                            }
                        },
                    },
                    series: [
                        // {
                        //     name: city,
                        //     type: 'map',
                        //     map: city,
                        //     zoom: 1.2,
                        //     data:this.address_info.map((item)=>{
                        //         return{
                        //             name:item.name,
                        //             value:item.abbr,
                        //             label:{
                        //                 normal: {
                        //                     show: true,
                        //                     color:'#f2f2f2',
                        //                     textStyle:{
                        //                         fontSize:11
                        //                     }
                        //                 },
                        //                 emphasis: {
                        //                     show:true,
                        //                     textStyle: {
                        //                         color: '#fff',
                        //                         fontSize:11
                        //                     }
                        //                 },
                        //             },
                        //             itemStyle: {
                        //                 normal: {
                        //                     borderColor: '#ffffff', //区域边框颜色
                        //                     areaColor: '#253752', //区域填充颜色
                        //                     color: {
                        //                         type: 'linear',
                        //                         x: 0,
                        //                         y: 0,
                        //                         x2: 0,
                        //                         y2: 1,
                        //                         colorStops: [{
                        //                             offset: 0, color: '#096dd9' // 0% 处的颜色
                        //                         }, {
                        //                             offset: 1, color: '#002766' // 100% 处的颜色
                        //                         }],
                        //                         global: false // 缺省为 false
                        //                     },
                        //                 },
                        //                 emphasis: {
                        //                     borderColor: '#ffffff', //区域边框颜色
                        //                     areaColor: '#0050b3',
                        //                 }
                        //             }
                        //         }
                        //     })
                        // },
                        {
                            name:"正常",
                            type: 'scatter',
                            coordinateSystem: 'geo',
                            showEffectOn: 'render',
                            showSymbol: true,
                            symbolSize:20,
                            symbol:"pin",
                            color:"#4caf50",
                            data: data.map(function (itemOpt) { //散点生成
                                if(itemOpt.abbr == 'lhs' || itemOpt.abbr == 'tmg')
                                    return;
                                if(itemOpt.is_normal != 1)
                                    return;
                                return {
                                    name: itemOpt.dirName,
                                    value: [
                                        itemOpt.longitude,
                                        itemOpt.latitude,
                                        itemOpt.stranger_num,//异常人数
                                        itemOpt.chef_people,//后厨人数
                                        itemOpt.food_people,//食品安全员人数
                                        itemOpt.damp_people,//超市人数
                                        itemOpt.health,//健康证数量
                                    ],
                                    // itemStyle: {
                                    //     normal: {
                                    //         color: '#4caf50'
                                    //     }
                                    // }
                                };
                            })
                        },
                        {
                            name:"异常",
                            type: 'scatter',
                            coordinateSystem: 'geo',
                            showEffectOn: 'render',
                            showSymbol: true,
                            symbolSize:20,
                            symbol:"pin",
                            color:"#d9001b",
                            data: data.map(function (itemOpt) { //散点生成
                                if(itemOpt.abbr == 'lhs' || itemOpt.abbr == 'tmg')
                                    return;
                                if(itemOpt.is_normal == 1)
                                    return;
                                return {
                                    name: itemOpt.dirName,

                                    value: [
                                        itemOpt.longitude,
                                        itemOpt.latitude,
                                        itemOpt.stranger_num,//异常人数
                                        itemOpt.chef_people,//后厨人数
                                        itemOpt.food_people,//食品安全员人数
                                        itemOpt.damp_people,//超市人数
                                        itemOpt.health,//健康证数量
                                    ],
                                    py:itemOpt.abbr,
                                    itemStyle: {
                                        normal: {
                                            color: '#d9001b'
                                        }
                                    }
                                };
                            })
                        }
                    ]
                },true);
            },
            showButton(){

                this.$emit("showButton", true);
            },
            getAreaInfo(city, indexCode, city_name){
                this.$emit("getAreaInfo", city, indexCode, city_name );
                this.showButton();
            },
        }
    }
</script>
<style scoped lang="less">
    .echarts{
        width: 100%;
        height: 100%;
        margin-top: -20px;
    }
</style>
