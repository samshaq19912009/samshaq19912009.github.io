---
layout: post
title: RNN 算法简介
date: 2016-07-10
categories: blog
tags: [Machine Learning, Notes]
description: RNN
---





RNN(Recurrent Neural Network)被运用到了很多自然语言处理（NLP）工作中，并且取得了非常好的效果。这里我们将要实现一个以RNN为基础的语言模型。这一语言模型有两个主要的用途，第一，根据任意给定的语句，给定其出现在固定位置的可能性，这一概率可以用来判断机器生成的句子的语法以及语义正确的概率，这一模型主要被应用在翻译系统中。 第二， 语言模型可以生成新的段落。 给定一段莎士比亚的文字，可以生成一段具有莎士比亚风格的段落。


## 什么是 RNN?

RNN最重要的思想，是利用具有顺序的信息。在传统的neural network中， 我们假定了每一个输入（输出）都是独立的，但是在实际应用中，这并不符合现实。例如，在一个句子里，我们需要根据之前的单词来预测下一个单词。这也就是RNN中**recurrent**的来源，在RNN model中，每一个都执行一样的任务，而每一次的输入都依赖于上一次运行的输出。另一种思考的方法是， RNN model是有记忆的， 在运行时依赖于已经得到的信息。下图是一个典型的RNN model 示意图。


![](http://d3kbpzbmcynnmx.cloudfront.net/wp-content/uploads/2015/09/rnn.jpg)

上图展示了一个RNN model 被展开之后的情形。 这里的展开指的是将所有的network展开。例如，如果这一系列是5个单词的句子，这时network就是一个5层的神经网络，每一个层代表一个Word。图中的公式意为：

* $x_t$是在time step t的输入，例如，$x_1$代表句中第二个单词的向量

* $s_t$是在time step t的隐藏变量， 代表之前提到的RNN model的“记忆”，  $s_t$由之前的隐藏变量和现在步骤的输入来计算， 记为 $s_t = f(U x_t + W s_{t-1} )$。 $s_{-1}$通常被初始化为零。

* $o_t$是在time step t的输出，当我们想要预测句中下一个单词的时候，将会输出词汇表中各个单词的概率： $o_t = softmax(V s_t)$

* $s_t$可以被当做RNN model的“记忆”来储存之前步骤中的信息。每一步的输出 $o_t$都只由同时的信息的来完成

* 另外，传统的深度学习网络，在每一层的网络都使用不同的参数，与之不同的是， RNN model 在每一步都使用同样的参数（U, V, W)。事实上，在每一步我们都采用相同的步骤，但使用不同的input。这可以极大的减少需要调节的参数的数目。


## RNN的用途

在很多自然语言处理任务中， RNN都有很成功的应用。其中，大部分的RNN model都是基于[LSTM](https://en.wikipedia.org/wiki/Long_short-term_memory)。以下介绍两种RNN的常见用途


### 语言模型以及语句生成

给定一语言序列，我们想要根据之前的语句来预测每一个单词生成的概率。语言模型可以来测定每一个句子的出现概率，从而被用来进行语言翻译（高概率的语句通常正确率更高）。通过预测下一个词句的概率，我们也可以得到一个生成模型，(generative model)，从而可以生成新的语句。根据我们的训练数据，我们可以生成**各种各样的词句**。 在语言模拟中， 模型的输入通常是一串词句，输出为一串预测的词句。


### 机器翻译

机器翻译和语言模型类似，输入为源语言（德语）的一串词句，我们想要输出目标语言（英语）的一串词句。最关键的不同点在于，只有在所有的input都完成训练后，才会开始输出， 因为输出的词句中，第一个单词可能会依赖于完整句子的语义。

![](http://d3kbpzbmcynnmx.cloudfront.net/wp-content/uploads/2015/09/Screen-Shot-2015-09-17-at-10.39.06-AM-1024x557.png)

### 语音识别

给定声波中的一串声音讯号， 我们可以来做出语音判断以及每一个判断输出的概率。

### 图像描述

与CNN 模型共同使用后，RNN被用来进行对图像进行描述。如下图所示。


![](http://d3kbpzbmcynnmx.cloudfront.net/wp-content/uploads/2015/09/Screen-Shot-2015-09-17-at-11.44.24-AM-1024x349.png)


## 训练 RNN model

训练一个RNN model和训练其他的深度学习网络类似，我们依然采用backpropagation算法，但也有不同。每一步的参数都会被所有的步骤所公用，在每一步的梯度(gradient)都不仅仅依赖于当前一步的计算，同时也依赖于前一步。例如，为了计算t=4的梯度，我们需要backpropagate前三步并且将所得的梯度加和。这一算法被称为 Backpropagation Through Time (BPTT).


## 语言模型

我们的目标是使用RNN来建议一个语言模型。假定，我们有m个单词，语言模型可以用来预测这m个单词的概率为

$
\begin{aligned}
P(w_1,...,w_m) = \prod_{i=1}^{m}P(w_i \mid w_1,..., w_{i-1}) 
\end{aligned}
$

在句子里， 每一个句子出现的概率等于句子中单词出现概率的乘积。例如， 出现句子“He went to buy some stuff”的概率等于句子“some stull”的概率乘以句子“He went to buy”的概率。

我们想要知道的是， 为什么这个有用呢？ 为什么对于句子我们需要给定一个概率呢？

首先，这一model可以当做为一种scoring mechanism。 例如， 在机器翻译中，给定一个输入的语句，系统通常会产生多个可能的输出。我们可以利用语言模型来选择其中最可能的语句，直观地来看，最有可能的语句最后有很大的可能是语法正确的。相似的scoring mechanism也被用作在语音识别中。

但是， 解决语言模型也同时带来了一个副作用。因为我们可以根据之前出现的词句来推断词句出现的概率，这样，我们就可以生成新的词句。给定已经生成的词句，我们可以从计算所得的概率进行采样，重复这一过程直到一个新的语句。

## 训练模型 预处理

为了训练我们的语言模型，我们需要文本来进行训练。幸运的是，我们只需要纯文本训练即可，无需任何的标记。。我们下载了15000个reddit网站上的评论，这样， 我们生成的文本就会跟reddit的评论相似。和大多数机器学习的项目一样，我们首先需要进行对数据的预处理。

### 记号化文本

我们需要基于词语来进行预测，因此，对于给定的纯文本，我们必须把评论记号化为句子，然后再把句子标记化为词语。这里， 使用NLTK的 word_tokenize 和 sent_tokenize

### 去除少见词语

大部分出现在文本中的词语只会出现少数的几次，显然在模型中将这些少见词语省略会极大的节约训练的时间。

在现有的model中， 我们把词汇表的数目限定在8000以下，即选定最长出现的8000个词语，把没有被选择进词汇表的词语都标记为UNKOWN_TOKEN.在训练的时候，这个UNKOWN_TOKEN也会被当做训练model中的一部分，在最终生成文本的时候，UNKOWN_TOKEN会被任意一个不在词汇表的词语所替代。


### 准备特殊开始结束记号

我们同时希望学习什么样的词句可以用来开始和结束一段语句。为此，我们准备了特殊的SENTENCE_START 标记 以及特殊的SENTENCE_END 标记。 这样， 给定了一个SENTENCE_START，下一个可能的词语是什么？

### 建立训练的矩阵

RNN model的输入为向量，并不是字符串。所以，我们采用词语和下标的配对:index_to_word 和 word_to_index。例如， 词语“friendly”可能在下标2001. 一个训练的例子x为[0, 179, 341, 146], 其中， 0代表开始语句SENTENCE_TO_START，对应的y为[179, 341, 146,1]。这里，我们需要预测的是下一个词语，所以y实际上就是将x右移了一位并且添加了最后一个SENTENCE_END，换句话来说，在上面的例子中，179的后一个词语，就是341。





### 建立RNN model

现在我们可以来看看 RNN model到底是什么样子的了。每一个输入的x将会是一串词语（就像之前的例子一样），而每一个$x_t$会是一个单词，这里有一点需要注意，我们并不能直接把词语转换成下标然后进行矩阵运算，而是将每一个单词做成一个one-hot的向量，而这个向量的大小，就是之前所提到的vocabulary的大小，例如，一个下标为36的单词，就是所有除了36这一位皆为0，只有36位为1的单词。这样，每一个$x_t$都是一个向量，而每一个x都会是一个矩阵。考虑这一神经网络的最后输出$o_t$也会是一个向量，其向量大小就是vocabulary_size。而每一个o都为一个矩阵。

接下来，我们回顾一下之前提到的RNN model的具体公式。

$
\begin{aligned}
s_t &= \tanh(Ux_t + Ws_{t-1}) \\
o_t &= \mathrm{softmax}(Vs_t)
\end{aligned}
$

在神经网络的学习中，将每一个变量的大小写出，是很有帮助的。这里，我们使用800作为vocabulary_size， 100作为hidden layer的大小，那么，训练中各变量的大小皆为以下所示：

$
\begin{aligned}
x_t & \in \mathbb{R}^{8000} \\
o_t & \in \mathbb{R}^{8000} \\
s_t & \in \mathbb{R}^{100} \\
U & \in \mathbb{R}^{100 \times 8000} \\
V & \in \mathbb{R}^{8000 \times 100} \\
W & \in \mathbb{R}^{100 \times 100} \\
\end{aligned}
$

值得注意的是， U, V, W都是在模型中需要训练的参量。因此，这一模型需要学习的参数总量则为$2HC + H^2$，在这一例子中H为100， C为800， 那么总计需要学习的变量为1,610,00。由此我们也可以看出，这一模型的局限性。

#### 初始化

为了定义我们的参数，我们首先定义一个RNN 的class。初始化U,V,W需要一点的技巧性，我们并不能把这些参数都初始化为0，这会导致每一层layer都会是一个对称的计算。这里，我们采用随机初始化的方式。在这方面已经有非常多的研究表明，参数初始化的方式依赖于activation function，[推荐的](http://jmlr.org/proceedings/papers/v9/glorot10a/glorot10a.pdf)一个方法是，对于tanh的activation function，初始化参数在区间 $\left[-\frac{1}{\sqrt{n}}, \frac{1}{\sqrt{n}}\right]$ 中，其中$n$ 为前一层layer的数目。

```Python
class RNNNumpy:
    
    def __init__(self, word_dim, hidden_dim=100, bptt_truncate=4):
        # Assign instance variables
        self.word_dim = word_dim
        self.hidden_dim = hidden_dim
        self.bptt_truncate = bptt_truncate
        # Randomly initialize the network parameters
        self.U = np.random.uniform(-np.sqrt(1./word_dim), np.sqrt(1./word_dim), (hidden_dim, word_dim))
        self.V = np.random.uniform(-np.sqrt(1./hidden_dim), np.sqrt(1./hidden_dim), (word_dim, hidden_dim))
        self.W = np.random.uniform(-np.sqrt(1./hidden_dim), np.sqrt(1./hidden_dim), (hidden_dim, hidden_dim))
        

``` 

#### Forward Propagation

跟其他neural network一样，我们采用 forward propogation 来预测输出。

```Python
def forward_propagation(self, x):
    # The total number of time steps
    T = len(x)
    # During forward propagation we save all hidden states in s because need them later.
    # We add one additional element for the initial hidden, which we set to 0
    s = np.zeros((T + 1, self.hidden_dim))
    s[-1] = np.zeros(self.hidden_dim)
    # The outputs at each time step. Again, we save them for later.
    o = np.zeros((T, self.word_dim))
    # For each time step...
    for t in np.arange(T):
        # Note that we are indxing U by x[t]. This is the same as multiplying U with a one-hot vector.
        s[t] = np.tanh(self.U[:,x[t]] + self.W.dot(s[t-1]))
        o[t] = softmax(self.V.dot(s[t]))
    return [o, s]

RNNNumpy.forward_propagation = forward_propagation


```

从以上的forward propogation中，我们不仅输出了o，并且输出了hidden layer的s。s将会被用来计算梯度。每一个$o_t$是一个代表词语出现在词汇表中的概率，为了最后预测，我们选取最高概率的词语。

```Python
def predict(self, x):
    # Perform forward propagation and return index of the highest score
    o, s = self.forward_propagation(x)
    return np.argmax(o, axis=1)

RNNNumpy.predict = predict

```


#### 计算损失函数

为了训练我们的model，我们需要定义一个测量错误率的metric，这就是我们的损失函数(loss function)$L$,而我们选择参数$U, V, W$来最小化损失函数。一个经常被选取的是交叉熵[cross-entropy loss](https://en.wikipedia.org/wiki/Cross_entropy#Cross-entropy_error_function_and_logistic_regression)。给定一共N个训练样本，y代表真实值，o代表模型预测值，C代表vocabulary的size，损失函数为：


$
\begin{aligned}
L(y,o) = - \frac{1}{N} \sum_{n \in N} y_{n} \log o_{n}
\end{aligned}
$


在具体训练模型之前，让我们来看看随机初始化的损失函数应该多少，这会给我们的训练带来一个基础值作为参考。在模型中，我们有C个vocabulary，所以每一个单词出现的概率就应该是1/C， 这样，最后的损失函数值应该是：$L = -\frac{1}{N} N \log\frac{1}{C} = \log C$。下面的例子中，我们给定前1000个样本，来看看我们设计的损失函数与随机初始化是否相符合。

```Python
# Limit to 1000 examples to save time
print "Expected Loss for random predictions: %f" % np.log(vocabulary_size)
print "Actual loss: %f" % model.calculate_loss(X_train[:1000], y_train[:1000])
```

运行后，预期的随机初始化的误差值为8.987197，通过计算所得的误差值为8.987440，还是非常接近的！！！

#### SGD and Backpropagation Through Time (BPTT)

我们的目标是，通过调节参数U,V,W来找到损失函数的最小值，最常见的方法，是通过SGD（Stochastic Gradient Descent)算法。SGD算法的思想实际上非常简单，我们遍历所有的训练样本，每一次遍历，我们根据减少损失函数的方向来优化参数。 这一方向由损失函数的梯度来给出：$\frac{\partial L}{\partial U}, \frac{\partial L}{\partial V}, \frac{\partial L}{\partial W}$。SGD算法同时需要一个learning rate来定义优化梯度的速率。SGD算法不仅是神经网络中非常常见的算法，也经常被使用在其他的机器学习算法中。关于SGD算法的具体补充，可以参考这个链接[地址](http://cs231n.github.io/optimization-1/)。

根据SGD算法，计算梯度成为非常重要的一步。在其他的神经网络中，我们采用backpropagation算法。在RNN model中，需要对backpropagation进行一些改动，称为BPTT(Backpropagation Through Time)算法。因为参数在每一层的运算中都是通用的，在每一步的输出中，梯度的计算并不只由当前的计算步骤决定，也跟之前的步骤决定。这里，我们只给出BPTT最后的算法：给定输入的x和y，给出梯度$\frac{\partial L}{\partial U}, \frac{\partial L}{\partial V}, \frac{\partial L}{\partial W}$。

```Python
def bptt(self, x, y):
    T = len(y)
    # Perform forward propagation
    o, s = self.forward_propagation(x)
    # We accumulate the gradients in these variables
    dLdU = np.zeros(self.U.shape)
    dLdV = np.zeros(self.V.shape)
    dLdW = np.zeros(self.W.shape)
    delta_o = o
    delta_o[np.arange(len(y)), y] -= 1.
    # For each output backwards...
    for t in np.arange(T)[::-1]:
        dLdV += np.outer(delta_o[t], s[t].T)
        # Initial delta calculation
        delta_t = self.V.T.dot(delta_o[t]) * (1 - (s[t] ** 2))
        # Backpropagation through time (for at most self.bptt_truncate steps)
        for bptt_step in np.arange(max(0, t-self.bptt_truncate), t+1)[::-1]:
            # print "Backpropagation step t=%d bptt step=%d " % (t, bptt_step)
            dLdW += np.outer(delta_t, s[bptt_step-1])              
            dLdU[:,x[bptt_step]] += delta_t
            # Update delta for next step
            delta_t = self.W.T.dot(delta_t) * (1 - s[bptt_step-1] ** 2)
    return [dLdU, dLdV, dLdW]

RNNNumpy.bptt = bptt


```


#### Gradient Checking

每次我们进行backprogation的运算中，进行*梯度检查（gradient checking）*是非常重要的，这可以让我们来确认梯度算法的正确性。梯度检查是根据梯度的运算结果与此刻该点的斜率类似， 如以下公式所示。

$
\begin{aligned}
\frac{\partial L}{\partial \theta} \approx \lim_{h \to 0} \frac{J(\theta + h) - J(\theta -h)}{2h}
\end{aligned}
$

接下来我们需要对计算所得的梯度进行gradient check，如果没有较大偏差的话，那么说明我们的算法是正确的。值得注意的是，这一检查需要计算每一个参数的梯度，在大多数情况下，这将极大的增加运算量。

```Python
def gradient_check(self, x, y, h=0.001, error_threshold=0.01):
    # Calculate the gradients using backpropagation. We want to checker if these are correct.
    bptt_gradients = model.bptt(x, y)
    # List of all parameters we want to check.
    model_parameters = ['U', 'V', 'W']
    # Gradient check for each parameter
    for pidx, pname in enumerate(model_parameters):
        # Get the actual parameter value from the mode, e.g. model.W
        parameter = operator.attrgetter(pname)(self)
        print "Performing gradient check for parameter %s with size %d." % (pname, np.prod(parameter.shape))
        # Iterate over each element of the parameter matrix, e.g. (0,0), (0,1), ...
        it = np.nditer(parameter, flags=['multi_index'], op_flags=['readwrite'])
        while not it.finished:
            ix = it.multi_index
            # Save the original value so we can reset it later
            original_value = parameter[ix]
            # Estimate the gradient using (f(x+h) - f(x-h))/(2*h)
            parameter[ix] = original_value + h
            gradplus = model.calculate_total_loss([x],[y])
            parameter[ix] = original_value - h
            gradminus = model.calculate_total_loss([x],[y])
            estimated_gradient = (gradplus - gradminus)/(2*h)
            # Reset parameter to original value
            parameter[ix] = original_value
            # The gradient for this parameter calculated using backpropagation
            backprop_gradient = bptt_gradients[pidx][ix]
            # calculate The relative error: (|x - y|/(|x| + |y|))
            relative_error = np.abs(backprop_gradient - estimated_gradient)/(np.abs(backprop_gradient) + np.abs(estimated_gradient))
            # If the error is to large fail the gradient check
            if relative_error > error_threshold:
                print "Gradient Check ERROR: parameter=%s ix=%s" % (pname, ix)
                print "+h Loss: %f" % gradplus
                print "-h Loss: %f" % gradminus
                print "Estimated_gradient: %f" % estimated_gradient
                print "Backpropagation gradient: %f" % backprop_gradient
                print "Relative Error: %f" % relative_error
                return 
            it.iternext()
        print "Gradient check for parameter %s passed." % (pname)

RNNNumpy.gradient_check = gradient_check

# To avoid performing millions of expensive calculations we use a smaller vocabulary size for checking.
grad_check_vocab_size = 100
np.random.seed(10)
model = RNNNumpy(grad_check_vocab_size, 10, bptt_truncate=1000)
model.gradient_check([0,1,2,3], [1,2,3,4])


```
#### SGD Implementation

现在我们可以开始完成SGD函数了。具体步骤分为两步，第一步，一个`sdg_step`函数用来计算梯度并且更新参数。第二步，遍历每一个训练样本，调整learning rate。

```Python
# Performs one step of SGD.
def numpy_sdg_step(self, x, y, learning_rate):
    # Calculate the gradients
    dLdU, dLdV, dLdW = self.bptt(x, y)
    # Change parameters according to gradients and learning rate
    self.U -= learning_rate * dLdU
    self.V -= learning_rate * dLdV
    self.W -= learning_rate * dLdW

RNNNumpy.sgd_step = numpy_sdg_step




# Outer SGD Loop
# - model: The RNN model instance
# - X_train: The training data set
# - y_train: The training data labels
# - learning_rate: Initial learning rate for SGD
# - nepoch: Number of times to iterate through the complete dataset
# - evaluate_loss_after: Evaluate the loss after this many epochs
def train_with_sgd(model, X_train, y_train, learning_rate=0.005, nepoch=100, evaluate_loss_after=5):
    # We keep track of the losses so we can plot them later
    losses = []
    num_examples_seen = 0
    for epoch in range(nepoch):
        # Optionally evaluate the loss
        if (epoch % evaluate_loss_after == 0):
            loss = model.calculate_loss(X_train, y_train)
            losses.append((num_examples_seen, loss))
            time = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
            print "%s: Loss after num_examples_seen=%d epoch=%d: %f" % (time, num_examples_seen, epoch, loss)
            # Adjust the learning rate if loss increases
            if (len(losses) > 1 and losses[-1][1] > losses[-2][1]):
                learning_rate = learning_rate * 0.5  
                print "Setting learning rate to %f" % learning_rate
            sys.stdout.flush()
        # For each training example...
        for i in range(len(y_train)):
            # One SGD step
            model.sgd_step(X_train[i], y_train[i], learning_rate)
            num_examples_seen += 1

```

完成了！ 现在我们可以对RNN model进行测试了。具体代码如下：

```Python
np.random.seed(10)
model = RNNNumpy(vocabulary_size)
%timeit model.sgd_step(X_train[10], y_train[10], 0.005)

```

这里我们可以看到， 一个训练样本需要200多ms来完成，而我们有80000个样本，这意味着完成训练需要长达几个小时，多次迭代则需要几天的时间。有很多种优化模型的方法，而使用GPU进行运算就是其中的一种。当然，我们可以先选择少量的样本来用CPU进行运算

```Python

np.random.seed(10)
# Train on a small subset of the data to see what happens
model = RNNNumpy(vocabulary_size)
losses = train_with_sgd(model, X_train[:100], y_train[:100], nepoch=10, evaluate_loss_after=1)
```

可以看到的是，经过每一步，损失函数都在减少，证明我们的model确实有了效用！

### Training our Network with Theano and the GPU

在向量运算中，如果使用Theano以及GPU运算，会极大的缩小运算的时间，这里我们不做过多的展开，直接参照之前的[tutorial](http://www.wildml.com/2015/09/speeding-up-your-neural-network-with-theano-and-the-gpu/)

```Python
from rnn_theano import RNNTheano, gradient_check_theano
np.random.seed(10)


# To avoid performing millions of expensive calculations we use a smaller vocabulary size for checking.


grad_check_vocab_size = 5
model = RNNTheano(grad_check_vocab_size, 10)
gradient_check_theano(model, [0,1,2,3], [1,2,3,4])
np.random.seed(10)


model = RNNTheano(vocabulary_size)
%timeit model.sgd_step(X_train[10], y_train[10], 0.005)

```

此时，我们可以发现，运算时间为50ms，以及缩小为之前的四分之一。接下来，我们就可以开始训练模型并且生成文本了。为了节约时间，我们载入之前训练好的模型参数。


```Python
from utils import load_model_parameters_theano, save_model_parameters_theano

model = RNNTheano(vocabulary_size, hidden_dim=50)
# losses = train_with_sgd(model, X_train, y_train, nepoch=50)
# save_model_parameters_theano('./data/trained-model-theano.npz', model)
load_model_parameters_theano('./data/trained-model-theano.npz', model)

```

生成文字的最终代码为：

```Python

def generate_sentence(model):
    # We start the sentence with the start token
    new_sentence = [word_to_index[sentence_start_token]]
    # Repeat until we get an end token
    while not new_sentence[-1] == word_to_index[sentence_end_token]:
        next_word_probs = model.forward_propagation(new_sentence)
        sampled_word = word_to_index[unknown_token]
        # We don't want to sample unknown words
        while sampled_word == word_to_index[unknown_token]:
            samples = np.random.multinomial(1, next_word_probs[-1])
            sampled_word = np.argmax(samples)
        new_sentence.append(sampled_word)
    sentence_str = [index_to_word[x] for x in new_sentence[1:-1]]
    return sentence_str

num_sentences = 10
senten_min_length = 7

for i in range(num_sentences):
    sent = []
    # We want long sentences, not sentences with one or two words
    while len(sent) < senten_min_length:
        sent = generate_sentence(model)
    print " ".join(sent)
```

我们列出几个输出的样本为

* no to blame their stuff go at all .

* consider via under gear but equal every game .

* no similar work on the ui birth a ce nightmare .

* the challenging what is absolutely hard .

* me do you research getting +2 .

* ugh is much good , no .

* me so many different lines hair .

* probably not very a bot or gain .

* correct this is affected so why ?

* register but a grown gun environment .

可以看到的是，这几个输出并不能做到符合语法和语义的双重满足，实际上，我们现有模型（Vanial RNN)最大的问题在于不能训练相隔几个单词内词语的关系。在接下来的分析中，我们会逐步介绍BPTT算法的详细步骤，以及在NLP中更常见的LSTMs模型。