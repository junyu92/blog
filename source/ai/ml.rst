Machine Learning
===============================

https://developers.google.com/machine-learning/crash-course/descending-into-ml/video-lecture

Framing
-------------------------------

监督学习
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

训练model的时候提供label

label
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

我们要预测的变量


features
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

描述输入的变量, 记作

.. math::

  \{x_1, x_2, \dot, x_n\}

Example
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

数据实例

- Labeled example 我们已知的example, 用来训练model, 表示为 

  .. math::
  
    \{features, label\}
- Unlabeled example 我们要预测的example, 表示为

  .. math::
  
    \{features, ?\}

Model
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A model defines the relationship between features and label.

- **Training** means creating or learning the model.
- **Inference** means applying the trained model to unlabeled examples.

Regression vs. classification
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- A **regression** model predicts continuous values.
- A **classification** model predicts discrete values.


Descending into ML
-------------------------------