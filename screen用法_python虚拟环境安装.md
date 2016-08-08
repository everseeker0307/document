####python 运行虚拟环境
+   pip3 install virtualenv
   
+   virtualenv env1, 创建env1虚拟环境
   
+   进入env1目录之后, 执行pip3 install flask(比如需要安装flask)
   
---

####screen 后台运行python程序
+   screen python3 xxx.py,  通过screen python3来执行程序

+   Ctrl+A+D, 退出当前screen窗口.  
    Ctrl+A+K, kill当前窗口.  
    Ctrl+A+W, 显示所有窗口列表.  

+   screen -ls, 列出当前所有session.  
    screen -r [id], 链接到后台窗口.
