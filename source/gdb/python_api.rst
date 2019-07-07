GDB
###############

python API
****************

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
===================
>>> (gdb) python print(42)
42

或者

>>> (gdb) python
>print(42)
>end
42

重要模块
==================

breakpoints
------------------
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

Parameter
-------------

使用python可以实现新的GDB parameter.

以内置的py脚本为例

.. code-block:: python

  import gdb
  import gdb.prompt
  
  class _ExtendedPrompt(gdb.Parameter):
  
      """Set the extended prompt.
  
  Usage: set extended-prompt VALUE
  
  Substitutions are applied to VALUE to compute the real prompt.
  
  The currently defined substitutions are:
  
  """
      # Add the prompt library's dynamically generated help to the
      # __doc__ string.
      __doc__ = __doc__ + gdb.prompt.prompt_help()
  
      set_doc = "Set the extended prompt."
      show_doc = "Show the extended prompt."
  
      def __init__(self):
          super(_ExtendedPrompt, self).__init__("extended-prompt",
                                                gdb.COMMAND_SUPPORT,
                                                gdb.PARAM_STRING_NOESCAPE)
          self.value = ''
          self.hook_set = False
  
      def get_show_string (self, pvalue):
          if self.value is not '':
             return "The extended prompt is: " + self.value
          else:
             return "The extended prompt is not set."
  
      def get_set_string (self):
          if self.hook_set == False:
             gdb.prompt_hook = self.before_prompt_hook
             self.hook_set = True
          return ""
  
      def before_prompt_hook(self, current):
          if self.value is not '':
              return gdb.prompt.substitute_prompt(self.value)
          else:
              return None
  
  _ExtendedPrompt()

``gdb.PARAM_*`` 指示了参数的类型，具体类型见文档

Variables:

set_doc
  ``set`` 的帮助信息

show_doc
  ``show`` 的帮助信息

value
  获得这个参数的值

Functions：

get_show_string
  在GDB中 ``show`` 该参数时，会调用这个函数

get_set_string
  在GDB中 ``set`` 该参数时，会调用这个函数

Function
===============

定义一个在gdb中可以使用的函数.

.. code-block:: python

  import gdb
  class CallF (gdb.Function):
      """Return True if the calling function's name is equal to a string.
  This function takes one or two arguments.
  The first argument is the name of a function; if the calling function's
  name is equal to this argument, this function returns True.
  The optional second argument tells this function how many stack frames
  to traverse to find the calling function.  The default is 1."""
  
      def __init__ (self):
          super (CallF, self).__init__ ("call_f")
  
      def invoke (self, name, nframes = 1):
          frame = gdb.selected_frame()
          print (frame.name())
          while nframes > 0:
              frame = frame.older()
              nframes = nframes - 1
          return frame.name () == name.string ()
  
  CallF()
  
在gdb中使用方面语句调用

>>> break foo if $call_f("main")

增加一个断点，当调用者的函数名称是 *main* 的时候，触发 *foo* 的断点.


Command
===============
要定义一个能在GDB中调用的Command，需要创建一个class

.. code-block:: python

  class HelloWorld (gdb.Command):
    """Greet the whole world."""
  
    def __init__ (self):
      super (HelloWorld, self).__init__ ("hello-world", gdb.COMMAND_USER)
  
    def invoke (self, arg, from_tty):
      print "Hello, World!"

  HelloWorld ()

在GDB中调用 *hello-world* 的时候, *invoke* 函数会被执行。

实现一个COMMAND能够保存断点
----------------------

下面实现一个COMMAND，把所有的断点保存到文件中。

.. code-block:: python

  import gdb
  
  class SaveBreakpointsCommand(gdb.Command):
      def __init__(self):
          super(SaveBreakpointsCommand, self).__init__("save-breakpoints",
                  gdb.COMMAND_SUPPORT,
                  gdb.COMPLETE_FILENAME)
  
      def invoke(self, arg, from_tty):
          with open(arg, 'w') as f:
              for bp in gdb.breakpoints():
                  print ("break " + bp.location, end=" ", file=f)
                  if bp.condition is not None:
                      print ("if " + bp.condition, end=" ", file=f)
                  print ("", file=f)
                  if not bp.enabled:
                      print ("disable $bpnum", file=f)
  
  SaveBreakpointsCommand()


Frame
===============

frame就是函数调用栈，当进入一个新的函数时，就会创建一个frame。

frame里面包含了传入的参数、局部变量以及正在执行的代码地址。

Blocks
===============

在GDB中，symbols保存在blocks中。

每个frame都有一个block

* global block
  记录了所有的全局变量和函数
* static block
  file-scoped的全局变量和函数

函数
----------------

gdb.block_for_pc
  根据pc或者block

GDB中有两种objects:

Symbols
===============

所有的variables, functions, types都是符号表中的一项。

Functions
---------------

gdb.lookup_symbol
  根据符号名找到symbol

gdb.lookup_global_symbol
  查找全局的符号

Variables
---------------

``gdb.Symbol``类型有下面的属性（部分）：

type
  符号的类型，该类型的返回值是``gdb.Type``

symtab
  返回符号表（类型gdb.Symtab）

line
  符号在哪行被定义

name
  符号的名称

needs_frame
  指示该symbol的value是否需要frame。典型的，如果是局部变量那么需要frame。

is_argument
  True if the symbol is an argument of a function.

is_constant
  True if the symbol is a constant.

is_function
  True if the symbol is a function or a method.

is_variable
  True if the symbol is a variable.

除了 ``gdb.Symtab`` 之前，还有一种类型为 ``gdb.Symtab_and_line`` 的符号表，它可以使用 ``find_sal`` 方法得到。