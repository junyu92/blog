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

Linear Regression
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Using a single straight line to approximate the relationship between
feature and label.

.. image:: image/CricketLine.svg

We can write down this relationship as follows:

.. math::

  y = w_1 x_1 +b

where:

- :math:`y` is the value we're trying to predict.
- :math:`w_1` is the weight of feature 1.
- :math:`x_1` is the value of our input feature.
- :math:`b` is the y-intercept(bias).

Training
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Training** a model simply means learning (determining) good values for all the
**weights** and the **bias** from labeled examples.

In supervised learning, a machine learning algorithm builds a model by examining
many examples and attempting to find a model that minimizes loss; this process is
called **empirical risk minimization**.

Loss
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Loss** is a number indicating how bad the model's prediction was on a single
example.

The goal of training a model is to find a set of weights and biases that have
low loss, on average, across all examples

L2-Loss
  Square of the different between predictio and label(true value).

  .. math::

    ( observation - prediction )^2

  When we are training a model, we care about minimizing loss accross
  our entire data set.

Mean square error(MSE)
  .. math::

    MSE = \frac{1}{N} \sum_{(x,y) \in D}{} (y - predication (x))^2

  where:

  - :math:`(x,y)` is an example in which

    - :math:`x` is the set of features (for example, chirps/minute, age, gender)
      that the model uses to make predictions.

    - :math:`y` is the example's label (for example, temperature).

  - :math:`prediction(x)` is a function of the weights and bias in combination
    with the set of features.

  - :math:`D` is a data set containing many labeled examples, which are :math:`(x,y)` pairs.

  - is the number of examples in :math:`D`.

Reducing Loss
-------------------------------

The learning continues iterating until the algorithm discovers the model parameters
with the lowest possible loss.

.. todo:

  未完成的章节

Gradient descent
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Convex problems have only one minimum; that is, only one place where the slope is exactly 0.
That minimum is where the loss function converges.

The first stage in gradient descent is to pick a starting value (a starting point) for :math:`w_1`.

The gradient descent algorithm then calculates the gradient of the loss curve at the starting point.
The gradient of loss is equal to the derivative (slope) of the curve, and tells you which way is
"warmer" or "colder." When there are multiple weights, the **gradient** is a vector of partial derivatives
with respect to the weights.

Learning Rate
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Gradient descent algorithms multiply the gradient by a scalar known as the **learning rate**
(also sometimes called **step size**) to determine the next point.

Goldilocks learning rate
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Stochastic gradient descent
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Mini-Batch gradient descent
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

