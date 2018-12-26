# Linux启动命令管理工具
---
- [x] 参考文献：
       - http://upstart.ubuntu.com/getting-started.html
       - http://blog.fens.me/linux-upstart/
       
       
- [x] Upstart特性:
       - upstart可以用来代替/etc/init.d/的执行脚本，额外提供了一些特性，像速度，状态检查，简单定义任务等。
       
         
![](/assets/upstart-state.png)

- 状态描述：

waiting : initial state.
starting : job is about to start.
pre-start : running pre-start section.
spawned : about to run script or exec section.
post-start : running post-start section.
running : interim state set after post-start section processed denoting job is running (But it may have no associated PID!)
pre-stop : running pre-stop section.
stopping : interim state set after pre-stop section processed.
killed : job is about to be stopped.
post-stop : running post-stop section.


