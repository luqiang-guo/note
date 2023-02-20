# GDB





## 调试汇编



b * mian  停到汇编层次

b main 听到c语言层次



```
display /i $pc  将要执行的汇编指令
display /3i $pc 3条指令

disassemble /m main
```



可视化  gdb ./xxx  -tui 



表示自动反汇编后面要执行的代码 set disassemble-next-line on

https://blog.csdn.net/hejinjing_tom_com/article/details/26704487







https://github.com/zhangyachen/zhangyachen.github.io/issues/134