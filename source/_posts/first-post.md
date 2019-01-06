---
title: first-post
date: 2019-01-07 02:17:44
tags:
    -nodejs
    -php
---

var express = require('express');
var app = express();
var session = require('express-session');
//设置一些初始化配置
//通过app.use中间件设置session的参数
app.use(session({
  name:'SESSIONID',
  secret:'@$#$^&%^$#%',
  cookie:{
    //设置session在cooklie会话加密
    path:'/',//根目录
    secure:false,//true只针对于https
    maxAge:1000*60*24,//24分钟
  }
}));

//设置session
app.get('/set',function(req,res){
  req.session.uid = 1111;
  req.session.username = 'admin';
  res.send('session已设置');
});

//获取session
app.get('/get',function(req,res){
  console.log(req.session.uid);
  console.log(req.session.username);
  res.send('获取session');
});

//删除一个session
app.get('/delui',function(req,res){
  req.session.username = null;
  res.send('删除username');
});

//删除整个session会话
app.get('/delsess',function(req,res){
  req.session.destroy(function(err){
    if (err) {
      throw err;
    }
  });
  res.send('删除session会话');
});

//有限期
app.get('/youxiaoqi',function(req,res){
  console.log(req.session.cookie.maxAge);
  
});
app.listen(3000,function(){
  console.log('http://127.0.0.1:3000/set');
  
})

