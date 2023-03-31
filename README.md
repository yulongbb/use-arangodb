# use-arangodb

如何使用arangodb开发应用

---

## 安装

docker run -d -p 8529:8529 -e ARANGO_ROOT_PASSWORD=root  arangodb/arangodb:3.8.0

访问 http://localhost:8529/

用户名: root

密码:root

## 导入数据

1. 创建GRAPH

进入GRAPHS页面，点击Add Graph，切换Examples, 选择Mps Graph点击Create,数据即初始化完毕。

2. 创建SERVICE

进入SERVICES页面，点击Add Service，切换New, 填写一下信息

* Author*: yulongbb
* Name*: mps
* Description*: mps
* License*: 1.0.0
* Document Collections: verts
* Edge Collections: edges

## 数据展示
