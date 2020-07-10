---
title: antd权限设计
date: 2019-10-25 14:39:12
tags: vue
---
# 一、概念
菜单权限: 每个角色能看见的菜单不一样
数据权限: 每个角色能进行的操作不一样(同一个页面,每个角色能看到的控件是不一样的)
每个角色对每个资源操作是不一样的
设计时就限定每个角色的数据权限

资源表信息
path 访问路径 /
name  描述 index
meta { title: '仪表盘', keepAlive: true, icon: bxAnaalyse, permission: [ 'dashboard' ] }
资源的名字,图标,是否缓存,权限标识
redirect 实际跳转路径
meta 的 permission 表示是否有此路由查看权限
<!-- more -->
# 二、表设计
## 用户表 account
```sql
CREATE TABLE "account" (
  "id" int(11) NOT NULL AUTO_INCREMENT COMMENT '主键自增id',
  "name" varchar(255) DEFAULT NULL COMMENT '昵称',
  "username" varchar(255) DEFAULT NULL COMMENT '登录名',
  "password" varchar(255) DEFAULT NULL COMMENT '登录密码',
  "avatar" varchar(255) DEFAULT NULL COMMENT '用户头像路径',
  "status" int(10) DEFAULT NULL COMMENT '状态标识,0锁定,1有效',
  "telephone" varchar(255) DEFAULT NULL COMMENT '手机号',
  "lastLoginIp" varchar(255) DEFAULT NULL COMMENT '最后登录ip',
  "lastLoginTime" datetime DEFAULT NULL COMMENT '最后登录时间',
  "creatorId" int(11) DEFAULT NULL COMMENT '创建人id',
  "createTime" datetime DEFAULT NULL COMMENT '创建时间',
  "merchantCode" varchar(255) DEFAULT NULL COMMENT '商品代码',
  "deleted" int(10) DEFAULT NULL COMMENT '删除标识 0有效,1删除',
  PRIMARY KEY ("id")
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

INSERT INTO `scott`.`account` 
(`id`, `name`, `username`, `password`, `avatar`, `status`, `telephone`, `lastLoginIp`, `lastLoginTime`, `creatorId`, `createTime`, `merchantCode`, `deleted`) 
VALUES 
('1', '天野远子', 'admin', '21232f297a57a5a743894a0e4a801fc3', 'https://gw.alipayobjects.com/zos/rmsportal/jZUIxmJycoymBprLOUbT.png', '1', '13888888888', '27.154.74.117', '2019-10-18 09:38:34', '1', '2019-10-18 09:38:51', 'TLif2btpzg079h15bk', '0');
```

## 角色表 role
```sql
CREATE TABLE "role" (
  "id" int(11) NOT NULL AUTO_INCREMENT COMMENT '主键id',
  "name" varchar(255) DEFAULT NULL COMMENT '权限名称',
  "nickname" varchar(255) DEFAULT NULL COMMENT '昵称',
  "describe" varchar(255) DEFAULT NULL COMMENT '权限说明',
  "status" int(10) DEFAULT NULL COMMENT '状态标识 0锁定 1有效',
  "creatorId" int(11) DEFAULT NULL COMMENT '创建人id',
  "createTime" datetime DEFAULT NULL COMMENT '创建时间',
  "deleted" int(10) DEFAULT NULL COMMENT '删除标识 0 有效 1 删除',
  PRIMARY KEY ("id")
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

INSERT INTO `scott`.`role` 
(`id`, `name`, `nickname`, `describe`, `status`, `creatorId`, `createTime`, `deleted`) 
VALUES 
('1', 'admin', '管理员', '拥有所有权限', '1', '1', '2019-10-18 09:39:54', '0');

```

## 用户角色关联表 account_role
```sql
CREATE TABLE "account_role" (
  "userId" int(11) DEFAULT NULL COMMENT '用户id',
  "roleId" int(11) DEFAULT NULL COMMENT '角色id'
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

INSERT INTO `scott`.`account_role` (`userId`, `roleId`) VALUES ('1', '1');

```

## 权限定义表 permission
```sql
CREATE TABLE "permission" (
  "id" int(11) NOT NULL AUTO_INCREMENT COMMENT '主键id',
  "permissionName" varchar(255) DEFAULT NULL COMMENT '权限描述',
  "permissionDesc" varchar(255) DEFAULT NULL COMMENT '描述',
  PRIMARY KEY ("id")
) ENGINE=InnoDB AUTO_INCREMENT=12 DEFAULT CHARSET=utf8;

INSERT INTO `scott`.`permission` (`id`, `permissionName`, `permissionDesc`) VALUES ('1', 'dashboard', '仪表盘');
INSERT INTO `scott`.`permission` (`id`, `permissionName`, `permissionDesc`) VALUES ('2', 'form', '表单权限');
INSERT INTO `scott`.`permission` (`id`, `permissionName`, `permissionDesc`) VALUES ('3', 'table', '表格权限');
INSERT INTO `scott`.`permission` (`id`, `permissionName`, `permissionDesc`) VALUES ('4', 'profile', '详细页权限');
INSERT INTO `scott`.`permission` (`id`, `permissionName`, `permissionDesc`) VALUES ('5', 'result', '结果权限');
INSERT INTO `scott`.`permission` (`id`, `permissionName`, `permissionDesc`) VALUES ('6', 'exception', '异常页面权限');
INSERT INTO `scott`.`permission` (`id`, `permissionName`, `permissionDesc`) VALUES ('7', 'user', '用户管理');
INSERT INTO `scott`.`permission` (`id`, `permissionName`, `permissionDesc`) VALUES ('8', 'support', '超级模块');
INSERT INTO `scott`.`permission` (`id`, `permissionName`, `permissionDesc`) VALUES ('9', 'order', '订单管理');
INSERT INTO `scott`.`permission` (`id`, `permissionName`, `permissionDesc`) VALUES ('10', 'permission', '权限管理');
INSERT INTO `scott`.`permission` (`id`, `permissionName`, `permissionDesc`) VALUES ('11', 'role', '角色管理');

```

## 操作列表 actions
```sql
CREATE TABLE "actions" (
  "id" int(11) NOT NULL AUTO_INCREMENT COMMENT '主键id',
  "action" varchar(255) DEFAULT NULL COMMENT '操作',
  "describe" varchar(255) DEFAULT NULL COMMENT '操作说明',
  PRIMARY KEY ("id")
) ENGINE=InnoDB AUTO_INCREMENT=8 DEFAULT CHARSET=utf8;

INSERT INTO `scott`.`actions` (`id`, `action`, `describe`) VALUES ('1', 'add', '新增');
INSERT INTO `scott`.`actions` (`id`, `action`, `describe`) VALUES ('2', 'get', '详情');
INSERT INTO `scott`.`actions` (`id`, `action`, `describe`) VALUES ('3', 'query', '查询');
INSERT INTO `scott`.`actions` (`id`, `action`, `describe`) VALUES ('4', 'update', '修改');
INSERT INTO `scott`.`actions` (`id`, `action`, `describe`) VALUES ('5', 'delete', '删除');
INSERT INTO `scott`.`actions` (`id`, `action`, `describe`) VALUES ('6', 'import', '导入');
INSERT INTO `scott`.`actions` (`id`, `action`, `describe`) VALUES ('7', 'export', '导出');

```

## 角色权限关联 role_permission
```sql
CREATE TABLE "role_permission" (
  "roleId" int(11) DEFAULT NULL COMMENT '角色id',
  "permissionId" int(11) DEFAULT NULL COMMENT '权限id',
  "actionId" int(11) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='表名角色拥有哪些菜单的查看权限';

INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '1', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '2', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '3', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '4', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '5', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '6', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '7', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '8', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '9', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '10', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '11', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '1', '2');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '1', '3');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '1', '4');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '1', '5');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '2', '2');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '2', '3');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '2', '4');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '3', '2');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '2', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '2', '5');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '3', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '3', '4');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '3', '6');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '6', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '6', '2');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '6', '3');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '6', '4');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '6', '5');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '5', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '5', '2');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '5', '3');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '5', '4');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '5', '5');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '4', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '4', '2');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '4', '3');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '4', '4');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '4', '5');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '9', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '9', '2');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '9', '3');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '9', '4');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '9', '5');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '10', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '10', '2');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '10', '4');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '10', '5');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '11', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '11', '2');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '11', '4');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '11', '5');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '7', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '7', '2');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '7', '4');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '7', '5');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '7', '6');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '7', '7');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '8', '1');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '8', '2');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '8', '4');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '8', '5');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '8', '6');
INSERT INTO `scott`.`role_permission` (`roleId`, `permissionId`, `actionId`) VALUES ('1', '8', '7');

```

# 三、流程
所有的菜单在前端定义好,后端不关心有哪些菜单
## 1.
登录获得服务器返回的用户token,保存到 storage (通过vue-ls)
将token 保存在 state user模块 的 token

## 2.
登录后跳转,触发 route.beforeEach方法,获取用户权限信息
如果store中没有用户的权限信息,则调用 action分发 GetInfo 取一次
使用 permissions.map 将操作 action 提取出成为一个数组

## 3.
判断 actionEntitySet是否为空
不为空, actionEntitySet.map,将 actionList 设置为 操作数组
将 permissoinList 设置为 权限数组

## 4.
用户权限 state->user->roles
用户信息 state->user->info
用户昵称 state->user->name
用户头像 state->user->avatar

## 5.
取到后,将用户权限当参数传递给 action分发 GenerateRoutes,生成动态路由

## 6.
将系统所有的菜单 和 用户权限传入 filterAsyncRouter
嵌套循环 有10项菜单,10个route
循环菜单,取出每一项 route

## 7.
将 permissionList 和 route 传入 hasPermission
循环 permissionList,看 route.meta.permission 是否包含 permissionList中的项
如果包含,则表示用户可以看到此菜单，递归调用,因为 route 会含有 childrens

## 8.
将包含的路由 添加到 accessRouters 
将 用户可访问的路由 保存至 state 的 addrouters

## 9.
用户可以访问的路由 = 基础路由 + addrouters
