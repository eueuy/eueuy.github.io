---
layout: post
title: When Code Grows (OOP)
author: zhangyue
---

* OOP基础思想
    * ADT
	* 函数
	* “一切都是对象”产生的问题
* 组织代码
    * 分治而产生类
        * 单一职责原则
	    * 封装
	    * 考虑抽象层级：包、接口
	    * 接口隔离原则
    * 考虑类之间通信
		* 通信产生耦合
            * 继承耦合
			* 组合耦合
			* 导入耦合
        * 低耦合通信的方法
	        * DI
			* Event Bus
			* 行为型设计模式
    * 考虑类的集成
			* 依赖倒置原则 
			* 创建型设计模式
			* 关注点分离
			* IOC
* 维护代码
    * 扩展手段
	    * 用来扩展代码的结构型设计模式
		* 多态、开放封闭原则
    * 复用手段
        * 继承、里氏替换原则
        * 组合
* 一些模式(PoEAA DDD)
    * 分层
        * 三层模型
        * 六边形模型
        * 服务层 Service Layer
    * 领域逻辑
        * 事务脚本
        * DDD
    * 数据访问
        * ORM
        * ActiveRecord
        * Repository

