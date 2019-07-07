Function
===========

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