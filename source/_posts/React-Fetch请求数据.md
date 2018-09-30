---
title: React Fetch请求数据
date: 2017-12-07 14:36:11
tags: React
---
## Fetch GET 请求
```
fetch("http://172.16.120.129:8080/travel/ticket/findTicketByBid.action?bid="+bussinessBid)
.then(res => res.json())  // json处理
.then(
    (result) => {
        console.log(result);  // json处理的数据              
    },
    (error) => {
        console.log(error);  //  错误信息
    }
)
```
## Fetch POST请求
```
//数据最好formdata
let formData = new FormData();
formData.append("username","admin");
formData.append("password","123456");
//请求
fetch("http://172.16.120.129:8080/travel//user/regist.action",{
    method:'POST', //请求方式
    body:formData, //请求数据
})/*.then(result =>{
    console.log(result);   //未通过json处理的
})*/
.then( (res) => res.json())
.then(
    (result) => {
        console.log(result);  // json处理的数据
    },
    (error) => {
        console.log(error);  //  错误信息
    }
)
```