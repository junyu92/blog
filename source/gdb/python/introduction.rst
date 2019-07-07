gdb python API
=============

参考：
http://sourceware.org/gdb/wiki/PythonGdbTutorial

使用gdb python API可以扩展gdb的功能，例如：

* 定义新的command
* pretty-printing
* 操作断点
* 访问stack frames
* 读写进程的内存
* etc.


直接在gdb中使用python
-------------
>>> (gdb) python print(42)
42

或者

>>> (gdb) python
>print(42)
>end
42

重要模块
-------------

breakpoints
##############
Breakpoint.location
  断点的位置

Breakpoint.condition
  断点的条件

Breakpoint.enabled
  断点是否启动，可写

函数
-------------

gdb.execute
  执行GDB的command

  >>> python gdb.execute('help')
  
gdb.breakpoints
  拿到GDB中设置的所有断点信息。

  >>> (gdb) b main
  Breakpoint 1 at 0x1249: file naughty.c, line 33.
  (gdb) b lazy_thread
  Breakpoint 2 at 0x11c5: file naughty.c, line 16.
  (gdb) python print(gdb.breakpoints())
  (<gdb.Breakpoint object at 0x7f6f0e19db20>, <gdb.Breakpoint object at 0x7f6f0e1c9788>)

gdb.rbreak
  拿到GDB中设置的所有断点信息

gdb.parse_and_eval(*expression*)
  计算 *expression* 的值

