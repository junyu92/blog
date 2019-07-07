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
