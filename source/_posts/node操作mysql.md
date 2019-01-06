---
title: node操作mysql
date: 2019-01-07 02:26:00
tags:
    -nodejs
    -mysql
---

var mysql = require('mysql');

//链接数据库
var connection = mysql.createConnection({
    host : '127.0.0.1',
    port : '3306',
    user : 'root',
    password : '123456',
    database : 'test'
});
//链接
connection.connect(function(err){
    if (err) {
        throw err;  
    }
    console.log('链接成功');
    
});

var controller = {};

//列表页
controller.index = function(req,res){
    //获取数据
    var sql = "select * from article";
    connection.query(sql,function(err,rows,fields){
        if (err) {
            throw err;
        }
        //分配模板
        res.render('index.html',{
            'rows':rows
        })
    })
}

//编辑入库
controller.upd = function(req,res){
    //接收参数
    var id = req.body.id;
    var title = req.body.title;
    var content = req.body.content;
    var sql = "update article set title=?,content=? where id =?";
    var bind = [title,content,id];
    connection.query(sql,bind,function(err,result){
        if (err) {
            throw err;
        }
        res.redirect('/');
    })
}

//删除
controller.del = function(req,res){
    var id = req.query.id;
    var sql = "delete from article where id =?";
    var bind = [id];
    connection.query(sql,bind,function(err,result){
        if (err) {
            throw err;
        }
        res.redirect('/');
    })
}

//编辑视图回显
controller.updView = function(req,res){
    var id = req.query.id;
    //查一条数据
    var sql = "select * from article where id=" +id;
    connection.query(sql,function(err,rows){
        if (err) {
            throw err;
        }
        //回显
        res.render('upd.html',{
            'row':rows[0]
        })
    })
}

//添加模板视图
controller.addView = function(req,res){
    res.render('add.html')
}

//post添加
controller.add = function(req,res){
    //接收post参数
    console.log(req.body);
    var title = req.body.title;
    var content = req.body.content;
    //入库参数
    var sql = "insert into article(title,content) values(?,?)";
    var bind = [title,content];
    connection.query(sql,bind,function(err,result){
        if (err) {
            throw err;
        }
        res.redirect('/');
    })
}

module.exports = controller;