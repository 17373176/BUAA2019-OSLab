# BUAA2019-OSLab3
## 进程
## 进程分配，进程调度
检查调用进程是否具有操作指定进程的合法权限。如果设置了A，
则指定的进程必须是当前进程或当前进程的直接子进程

伪指令：
.set伪指令(mips)
.set push --> save all settings 

.set reorder/noreorder --> let/don't let assembler reorder instructions 
.set at/noat --> let/don't let assembler use the register $at in instruction aliases (li,la, etc.)

.set pop --> restore saved settings 


.extern XXXX 说明xxxx为外部函数，调用的时候可以遍访所有文件找到该函数并且使用它。
.globle xxxxx 说明xxxx可以被其他文件调用，跟c语言里的全局变量的性质差不多吧。

## 课上测试
sched调度函数