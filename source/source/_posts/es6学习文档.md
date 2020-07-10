---
title: es6学习文档
date: 2019-10-25 14:31:25
tags: es6
---
[阮易峰es6](http://es6.ruanyifeng.com/#docs/destructuring)

```html
<!DOCTYPE html>
<html>
<head>
<title>array.map</title>
<meta charset="UTF-8">
<script>
```
<!-- more -->
```html
var actions = [{
        'action': 'add',
        'describe': '新增',
        'defaultCheck': false
      }, {
        'action': 'import',
        'describe': '导入',
        'defaultCheck': false
      }, {
        'action': 'get',
        'describe': '详情',
        'defaultCheck': false
      }, {
        'action': 'update',
        'describe': '修改',
        'defaultCheck': false
      }, {
        'action': 'delete',
        'describe': '删除',
        'defaultCheck': false
      }, {
        'action': 'export',
        'describe': '导出',
        'defaultCheck': false
      }];
	  
	  var ar = {
        'action': 'add',
        'describe': '新增',
        'defaultCheck': false
      };
	  
	  var meta = { title: '仪表盘', keepAlive: true, permission: [ 'dashboard' ] };
	  
	  window.onload=function(){
				var action = actions.map(action=>{
				return action.action
		})
		a({ar});
		
		var str="dashboard";
		console.log(meta.permission.includes(str));
				var {action} = ar;
				console.log(action);
				console.log({action});
	  }
	  
	  function a(data){
				var {ar} = data;
				console.log(ar.action);
	  }
	  
</script>
</head>
<body>
</body>
</html>
```