群聊聊天室

需求分析 ： 干什么  原型
【1】 有人进入聊天室需要输入姓名，姓名不能重复

【2】 有人进入聊天室时，其他人会收到通知：xxx 进入了聊天室

【3】 一个人发消息，其他人会收到：xxx ： xxxxxxxxxxx

【4】 有人退出聊天室，则其他人也会收到通知:xxx退出了聊天室

技术分析 ： 
 
	      1. 服务端需要存储聊天室用户  {name:address}
                                   [(name,address),]
                                class User:
                                    def __init__(self,name,address):
                                        self.name = name
                                        self.address = address
          2. 网络通信技术 ：  udp

          3. 消息发送 ： 使用转发机制

          4. 收发消息问题 ： 收发互不干扰 --》 使用不同的进程

功能模块分析

    函数封装

    整体结构的搭建
    进入聊天室
    聊天
    退出聊天室

通信协议分析

    网络通信协议： 让通信双方明白对方的数据含义

                请求类型    数据参量
    进入聊天室 ：    L        name

    聊天 ：         C       name  content

    退出聊天室 ：    Q



分模块具体逻辑梳理

    整体结构的搭建
       服务端：  1. 创建udp网络通信
               2. 准备循环接收客户端请求信息
               3. 根据请求，调用相关模块

       客户端： 1. 创建udp套接字


    进入聊天室
       客户端 ： 1. 输入用户名
               2. 发送用户名
               3. 等待结果
               4. 进入聊天室/ 重新输入姓名

       服务端 ： 1. 接收用户名
                2. 判断用户是否存在
                3. 存在-》不能进  不存在-》能进
                4. Yes 将用户信息存储起来
                5. 给其他人发送通知

    聊天
       客户端 ： 创建子进程
                子进程循环的接收消息
                父进程循环输入消息发送内容

       服务端 ： 接收消息
                将消息转发给其他人


    退出聊天室


cookie

   代码的总分处理模式
       × 在一处接收客户端发送来的所有请求消息
       × 根据消息请求类型的不同分情况处理
