---
title: Express+MongBD
date: 2018-04-19 13:48:10
tags:
---
## Express 
基于nodejs，快速、开发、极简的web开发框架
// 安装
```
npm install express --save
```
// 导入
```
const express = require('express');
app.get app.post 分别开发get和post接口
app.use使用模块
res.send res.json res.sendfile响应不同的内容
```
### psot请求
`npm install body-parser --save`
```
const bodyParser = require('body-parser');
const app = express();
app.use(bodyParser.json());
app.post('/register',function(req,res){
	console.log(req.body)
	const { user, password, type } = req.body;
	User.findOne({user:user},function(err,doc){
		if(doc){
			return res.json({code:1,msg:'用户名重复'})
		}
		User.create({user,password,type},function(err,doc){
			if(err){
				return res.json({code:1,msg:'后端出错了'})
			}
			return res.json({code:0})
		})
	})
})

```
### 密码加密
`npm install utility --save`
[utility文档](https://github.com/node-modules/utility)

## MongoDB
### 1.下载[MongoDB](https://www.mongodb.org/dl/win32/x86_64-2008plus-ssl?_ga=2.255015816.1613166733.1512467058-651293207.1512184673)解压版安装版都可以 
如果安装版
![](http://upload-images.jianshu.io/upload_images/2088736-9fd5ab25cdd1a37b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2.然后在安装目录下新建data/db、logs、配置项
这些是需要我们手动创建
data/db 存放数据库
logs/mongo.log 存放日志
![image.png](http://upload-images.jianshu.io/upload_images/2088736-aa134c052ba18ac7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
mongo.conf最重要可以加上配置
```
dbpath=D:\MongoDB3.4\data\db #数据库路径  
logpath=D:\MongoDB3.4\logs\mongo.log #日志输出文件路径  
logappend=true #错误日志采用追加模式  
journal=true #启用日志文件，默认启用  
quiet=true #这个选项可以过滤掉一些无用的日志信息，若需要调试使用请设置为false  
port=27017 #端口号 默认为27017  
```
然后在用命令行进入MongoDB3.4/bin下，执行
```
mongod --config "D:\MongoDB3.4\mongo.conf"
```
![image.png](http://upload-images.jianshu.io/upload_images/2088736-a9b9074700cb23e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后浏览器中输入
```
http://127.0.0.1:27017/
```
![image.png](http://upload-images.jianshu.io/upload_images/2088736-2d978260d3898bfc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
说明安装成功

### 创建并启动MongoDB
```
mongod --config "D:\MongoDB3.4\mongo.conf" --install --serviceName "MongoDB"
```
卸载服务
```
mongod.exe --remove --serviceName "MongoDB" 
```
![image.png](http://upload-images.jianshu.io/upload_images/2088736-108c16eaa500e9da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
此时可以使用`net start MongoDB`启动服务
![image.png](http://upload-images.jianshu.io/upload_images/2088736-8df3aa207ca987d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 程序中连接MongoDB
```
const mongoose = require('mongoose');
//连接mongoose 并且使用imooc这个集合
const DB_URL = 'mongodb://127.0.0.1:27017/jobapp';
mongoose.connect(DB_URL);
mongoose.connection.on('connected', function(){
	console.log('mongo connect success');
})
// 类似于mysql的表  mongo里面有文档、字段的概念
```
如果出现这样错误：
![image.png](http://upload-images.jianshu.io/upload_images/2088736-4a945ac4f328d620.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

连接方式改为这样`mongoose.connect(DB_URL,{useMongoClient: true});`

### MongoDB中增删改查
```
create 新增数据
remove 删除数据
update 修改(更新)数据
find,findOne 查找数据


```
#### 新增数据
```
const User = mongoose.model('user',new mongoose.Schema({
	user:{type:String,require:true},
	age:{type:Number,require:true}
}))
User.create({
	user: 'syymo',
	age:18
},function(err, doc){
	if(!err){
		console.log(doc);
	}else{
		console.log(err);
	}
})
```

#### 删除数据
```
User.remove({age:18},function(err,doc){
	if(!err){
		console.log('delete success');
		User.find({},function(e,d){
			console.log(d);
		})
	}
})
```
![image.png](http://upload-images.jianshu.io/upload_images/2088736-7342c0650f853da9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 修改数据
```
User.update({'user':'lml'},{'$set':{age:21}},function(err,doc){
	if(!err){
		console.log('uodate success');
		User.find({'user':'lml'},function(e,d){
			console.log(d)
		});
	}
})
```
#### 查询数据
```
// {} 查询所有
app.get('/data',function(req,res){
	User.find({}, function(err,doc){
		res.json(doc);	
	})
})
```

## Axios请求转发
在package.json中增加`"proxy":"http://localhost:9093" `
![image.png](http://upload-images.jianshu.io/upload_images/2088736-061448074e323aff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

端口为我们启动**后台服务**的端口
```
import axios from 'axios'
class Auth extends React.Component{
	componentDidMount(){
		axios.get('/data')
			.then(res=>{
				console.log(res);
			})
	}
	render(){
		return (
			<div>
				请求转发
			</div>
		)
	}
}
export default Auth;
```
Axios拦截器
- Axios.interceptors设置拦截器，比如全局的loading
- axios.get .post发送请求，返回promise对象
- Redux里获取数据，然后dispatch即可


## 基于cookie用户验证
- `express`以来cookie-parser，需要npm install cookie-parser --save安装
- cookie类似于一张身份证，登录后服务器端返回，你带着cookie就可以访问受限资源
- 页cookie的管理浏览器会自动处理

## 清除cookie
- 下载`npm install browser-cookies --save`
- 导入`import browserCookie from 'browser-cookies';`
- 擦除cookie`browserCookie.erase('userid')`
