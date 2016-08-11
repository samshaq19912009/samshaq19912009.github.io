---
layout: post
title: CNN 读书笔记（4） Batch Normalization
date: 2016-08-02
categories: blog
tags: [Machine Learning,Notes, CNN]
description: CNN

---




## Batch Normalization

### **Inutition**:

Machine learning methods tend to work better when their input data consists of uncorrelated features with zero mean and unit variance.

### **Problem**:

Even if we preprocess the input data, the activations at deeper layers of the network will likely no longer be decorrelated and will no longer have zero mean or unit variance since they are output from earlier layers in the network.

### **Methods**:

At training time, a **batch normalization layer** uses a minibatch of data to estimate the mean and standard deviation of each feature. These estimated means and standard deviations are then used to center and normalize the features of the minibatch. A **running average of these means** and standard deviations is kept during training, and at test time these running averages are used to center and normalize features.

### Show the code:

```Python
def batchnorm_forward(x, gamma, beta, bn_param):
  """
  Forward pass for batch normalization.

  During training the sample mean and (uncorrected) sample variance are
  computed from minibatch statistics and used to normalize the incoming data.
  During training we also keep an exponentially decaying running mean of the mean
  and variance of each feature, and these averages are used to normalize data
  at test-time.

  At each timestep we update the running averages for mean and variance using
  an exponential decay based on the momentum parameter:

  running_mean = momentum * running_mean + (1 - momentum) * sample_mean
  running_var = momentum * running_var + (1 - momentum) * sample_var
  Input:
  - x: Data of shape (N, D)
  - gamma: Scale parameter of shape (D,)
  - beta: Shift paremeter of shape (D,)
  - bn_param: Dictionary with the following keys:
    - mode: 'train' or 'test'; required
    - eps: Constant for numeric stability
    - momentum: Constant for running mean / variance.
    - running_mean: Array of shape (D,) giving running mean of features
    - running_var Array of shape (D,) giving running variance of features

  Returns a tuple of:
  - out: of shape (N, D)
  - cache: A tuple of values needed in the backward pass
  """
  mode = bn_param['mode']
  eps = bn_param.get('eps', 1e-5)
  momentum = bn_param.get('momentum', 0.9)

  N, D = x.shape
  running_mean = bn_param.get('running_mean', np.zeros(D, dtype=x.dtype))
  running_var = bn_param.get('running_var', np.zeros(D, dtype=x.dtype))

  out, cache = None, None
  if mode == 'train':
      sample_mean = np.mean(x, axis=0)
      sample_var  = np.var(x, axis=0)

      running_mean = momentum * running_mean + (1 - momentum) * sample_mean
      running_var = momentum * running_var + (1 - momentum) * sample_var

      x_normed = (x - sample_mean) / np.sqrt(sample_var + eps)
      y = x_normed * gamma + beta

      out = y

      cache = (x, x_normed, gamma, beta, sample_mean, sample_var, eps)

  elif mode == 'test':
      x_normed = (x - running_mean) / np.sqrt(running_var + eps)
      y = gamma * x_normed + beta

      out = y
  else:
      raise ValueError('Invalid forward batchnorm mode "%s"' % mode)

  # Store the updated running means back into bn_param
  bn_param['running_mean'] = running_mean
  bn_param['running_var'] = running_var

  return out, cache

```
### Backprogogation in Batch Normalization

```Python

def batchnorm_backward(dout, cache):
  """
  Backward pass for batch normalization.


  Inputs:
  - dout: Upstream derivatives, of shape (N, D)
  - cache: Variable of intermediates from batchnorm_forward.

  Returns a tuple of:
  - dx: Gradient with respect to inputs x, of shape (N, D)
  - dgamma: Gradient with respect to scale parameter gamma, of shape (D,)
  - dbeta: Gradient with respect to shift parameter beta, of shape (D,)
  """

  x, x_normed,gamma,beta, mean, var, eps = cache

  N = x.shape[0]


  dbeta = np.sum(dout, axis=0)
  dgamma = np.sum(dout * x_normed, axis=0)
  dx_normed = gamma * dout
  dx_std = np.sum(-1.0/2*dx_normed*(x-mean)  / (var+eps)**(3.0/2), axis=0)
  dx_mean = np.sum(dx_normed * -1.0 / np.sqrt(var+eps), axis=0) + 1.0/N * dx_std*np.sum(-2*(x - mean), axis=0)

  dx = dx_normed*1/np.sqrt(var+eps) + dx_std * 2.0/N*(x-mean)+ 1.0/N * dx_mean

  return dx, dgamma, dbeta

```

