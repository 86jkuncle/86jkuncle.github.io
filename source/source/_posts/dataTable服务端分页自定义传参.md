---
title: dataTable服务端分页自定义传参
date: 2020-05-15 10:16:26
tags: dataTable
---
1.关键代码,需要提前在html里定义好表格的标题,严格的table标签
```
<table id="table">
	<thead>
		<th>昵称</th>
		<th>头像</th>
		<th>姓名</th>
		<th>性别</th>
	</thead>
	<tbody>
	</tbody>
</table>
```
===
```
<script type="text/javascript">
	//封装成方法,可以将想要传入服务器的值传入此方法,不限个数
    function searchTableData(nickname,type_,openid,mobile,startdate,enddate) {
		if ($.fn.dataTable.isDataTable('#table'))
		{
			_table = $('#table').DataTable();
			_table.destroy();
		}
		var table=$('#table').DataTable({
			"lengthMenu": [10, 25, 50, 75, 100],
			"pageLength": 10,
			"stripeClasses": ["odd", "even"],
			"olanguage" : {
				"processing" : "处理中...",
				"lengthMenu" : "每页显示 _MENU_ 条记录",
				"zeroRecords" : "没有检索到数据",
				"info" : "从 _START_ 到 _END_ /共 _TOTAL_ 条数据",
				"infoEmpty" : "没有数据",
				"search" : "搜索",
				"infoFiltered" : "(从 _MAX_ 条数据中检索)",
				"paginate" : {
					"first" : "首页",
					"previous" : "前一页",
					"next" : "后一页",
					"last" : "尾页"
				}
			},
			"paginate" : true,//开启分页
			"pagingType":"simple_numbers",//分页的显示方式
			"destroy":true,
			"info":false,
			"lengthChange":true,
			"searching":false,
			"processing":true,
			"serverSide":true,//这个选项代表服务端分页,数据量大会一页一页查,这样就不会卡页面
			"ordering":false,
			"ajax":function(data,callback,setting){
                debugger;
                //封装请求参数
                var param = {};
                param.limit = data.length;//页面显示记录条数，在页面显示每页显示多少项的时候
                param.start = data.start;//开始的记录序号
                param.page = (data.start / data.length)+1;//当前页码
                param.startDate=startdate;
                param.endDate=enddate;
                param.nickname=nickname;
                param.type_=type_;
                param.openid=openid;
                param.mobile=mobile;
                //console.log(param);
                //ajax请求数据
                $.ajax({
                    type: "POST",
                    url: "${ctx}/search",
                    cache: false,  //禁用缓存
                    data: param,  //传入组装的参数
                    dataType: "json",
                    success: function (result) {
                        //console.log(result);
						//result是一个json 有四个值
						data,表格中需要显示的数据,是一个对象数组
						draw,请求次数,请求的时候传过来的,需要再传回
						recordsTotal,总的记录数
						recordsFiltered,文档上说是筛选的记录数,实际上这个的值也是总记录数,否则只能看到一页的数据,真吭
						
						//传给dataTable的回调函数
                        callback(result);
                    }
                });
            },
            "columns": [
                { data: 'nick_name' },
                { 
					data: 'head_pic' ,
					"render": function (data, type, row, meta) {
						return '<img alt="image" class="img-circle" src="'+data+'" style="width:50px;"/>';
                    }
				},
                { 
					data: 'name' ,
					"render": function (data, type, row, meta) {
						if(data == null){
                            return '';
						}else{
                            return data;
						}
					}
				},
				{ 
					data: 'sex',
					"render": function (data, type, row, meta) {
                        if(data == 1){
                            return '男';
                        }else{
							return '女';
                        }
                    }
				},
                {
					"render":function(data,type,row,meta){
                        return '<button class="btn btn-warning" onclick="enable(\'' +row.id+ '\')">设置</button>';
                    }
				}
			]
		});
	}
</script>
```
<!-- more -->
2.在第3页更新了数据,如果重载页面会跳到第一页,可以使用
```
$('#table').DataTable().draw()
```
只刷新当前页的数据,用户体验会有提升

3. .DataTable()获取的是原生DataTable对象, .dataTable()获取的是JQuery对象
jquery.api()转成原生对象,DataTable.to$()