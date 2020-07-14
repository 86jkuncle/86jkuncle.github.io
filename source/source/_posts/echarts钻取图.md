---
title: echarts钻取图
date: 2020-07-14 15:47:18
tags: echarts
---
```
<script>
    function isEmpty(obj){
        if(obj == undefined || obj == null || obj == '' || obj.length<0){
            return true;
        }else{
            return false;
        }
    }
	    
    </script>
	<script>
	var table = null;
	$(document).ready(function(){
		var start = {
				elem : '#startTime',
				 type:"date",
				  format: 'yyyy-MM-dd'
			
			};
			var end = {
				elem : '#endTime',
				 type:"date",
				  format: 'yyyy-MM-dd'
		
			};
			laydate.render(start);
			laydate.render(end);
	});
	
	</script>
	<script type="text/javascript">
	    // 基于准备好的dom，初始化echarts实例
		var myChart = echarts.init(document.getElementById('myEcharts'),"macarons");
        var sending = false;
        var firstData = null;
   		function query(){
   		    if(sending){
   		        return;
   		    }
   		    firstData = null;
   		    myChart.clear();
   			var type_ = $("#type_").val();
   			var startTime = $("#startTime").val();
   			var endTime = $("#endTime").val();
   			if(isEmpty(type_)){
   				layer.tips("请选择一个品类","#type_");
   				return false;
   			}

   			if(isEmpty(startTime)){
   				layer.tips("开始时间不能为空","#startTime");
   				return false;
   			}
   			if(isEmpty(endTime)){
   				layer.tips("结束时间不能为空","#endTime");
   				return false;
   			}
   			
   			if(Date.parse(startTime)>Date.parse(endTime)){
   				layer.tips("开始时间不能大于结束时间","#endTime");
   				return false;
   			}

   			if(Math.floor((Date.parse(endTime)-Date.parse(startTime)) / (24 * 3600 * 1000)) > 30){
   			    layer.tips("时间不能大于30天","#endTime");
   			    return false;
   			}

            sending = true;
            myChart.showLoading({text:"正在努力加载数据ing..."});
   			$.ajax({
				type:'POST',   				
   				url : '${ctx}/redenv/thermogram',
   				data:{
   					type:type_,
   					startTime:startTime,
   					endTime:endTime,
   				},
   				dataType:"json",
   				success : function(data){
   				    sending = false;
   				    myChart.hideLoading();
   				    if(data.province.length == 0){
   				    	layer.msg("没有数据");
   				    	return;
   				    }

                    firstData = data;
                    registerMap()
   				},
   				error:function(){
   				    sending = false;
   					layer.msg("服务器繁忙,请稍后再试!");
   				}
   			});		
   		}
	</script>
	<script>
	function registerMap(){
        rendHEAD();
        // 点击事件
        myChart.on('click', $mod => {
            console.log($mod)
            const { name, id } = $mod.data
            switch ($mod.data.zoom) {
                case 1:
                    rendProvince(name,id)
                    break
                case 2:
                    rendHEAD()
                    //rendCity(name, id)
                    break
                case 3: {
                    const oneMap = {
                        type: 'FeatureCollection',
                        features: [$mod.data.data],
                        parent: id,
                        zoom: 4
                    }
                    echarts.registerMap(name, oneMap)
                    renderMap(name, [{
                        name: name,
                        parent: id,
                        zoom: 4
                    }])
                    //this.setHashMap($mod.data.zoom + 1, name, value)
                }
                break
                case 4:
                    rendHEAD()
                    //clearHashMap()
                    break
            }
        })
    }

    function rendHEAD () {
        const mapName = []
        $.getJSON('${ctx}/map/china.json',function(data){
            data.features.forEach(province => {
                // 城市名称信息
                mapName.push({
                  // 名称
                    name: province.properties.name,
                      // 城市编码，对应文件名
                    id: province.id,
                      // 层级
                    zoom: 1,
                    value: 0
                })
            })

            //console.log(mapName)

            for(var i = 0;i < mapName.length;i++){
                for(var j = 0;j < firstData.province.length;j++){
                    if(mapName[i].name == firstData.province[j].name){
                        mapName[i].value = firstData.province[j].value
                        break;
                    }
                }
            }
            // 注册
            echarts.registerMap('中国', data)
            // 加载
            renderMap('china', mapName);
        })
    }

    function rendProvince (name, value) {
        var provinceFileName = '${ctx}/map/province/'+value+'.json'
        $.getJSON(provinceFileName,function(data){
            echarts.registerMap(name,data);
            var cityName = []
            data.features.forEach(city=>{
                cityName.push({
                    name:city.properties.name,
                    id: city.id,
                    data: city,
                    parent: value,
                    zoom: 2,
                    value: 0
                })
            })

            for(var i = 0;i < cityName.length;i++){
                for(var j = 0;j < firstData.city.length;j++){
                    if(cityName[i].name == firstData.city[j].name){
                        cityName[i].value = firstData.city[j].value
                        break;
                    }
                }
            }
            renderMap(name,cityName);
        })
    }

    function rendCity (name, value) {
        // name代表的是该地区中文名称
        // value 代表行政编号，如：3600000
        var cityFileName = './map/city/'+value+'.json'
        $.getJSON(cityFileName,function(data){
            echarts.registerMap(name, data)
            // 县区名称配置
            const countyName = []
            data.features.forEach(county => {
                countyName.push({
                    name: county.properties.name,
                    value: county.id,
                    data: county,
                    parent: value,
                    zoom: 3
                })
            })
            // 加载地图
            renderMap(name, countyName)
        })
    }

    function renderMap(mapName, cityNameList) {
        option = {
            animation:false,
            tooltip : {
                triggerOn:"mousemove",
                showContent:true,                           //是否显示提示框浮层
                alwaysShowContent:true,
                showDelay:0,                                  //浮层显示的延迟，单位为 ms,在 triggerOn 为 'mousemove' 时有效
                hideDelay:100,                                //浮层隐藏的延迟，单位为 ms
                enterable:false,                             //鼠标是否可进入提示框浮层中,默认为false
                renderMode: 'html'
            },
            visualMap: {
               show : true,
               x: 'left',
               y: 'center',
               splitList: [
                   {start: 30000, end:1000000},{start: 15000, end: 30000},
                   {start: 9000, end: 15000},{start: 3000, end: 9000},
                   {start: 1000, end: 3000},{start: 1, end: 1000},
               ],
               color: ['#5475f5', '#9feaa5', '#85daef','#74e2ca', '#e6ac53', '#9fb5ea']
           },
           series: [{
               name:mapName,
               type: 'map',
               mapType: mapName,
               label: {
                   normal: {
                       show: true,
                   },
                   emphasis: {
                       show: true,
                   }
               },
               data: cityNameList
           }]
        }
        //myChart.setOption(option);
        if (option && typeof option === "object") {
            myChart.setOption(option, true);
        }

        window.onresize = function() {
            myChart.resize(); //自适应
        }
    }
	</script>
```