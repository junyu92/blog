Command
-------------
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
^^^^^^^^^^^^^^^^

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
