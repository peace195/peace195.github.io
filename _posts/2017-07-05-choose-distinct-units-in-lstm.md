---
title: Choose some distinct units inside the recurrent (LSTM, GRU, ...) layer of recurrent neural networks
---

When working with a [recurrent neural networks](http://www.wildml.com/2015/09/recurrent-neural-networks-tutorial-part-1-introduction-to-rnns/) model, we usually use the last unit or some fixed units of recurrent series to predict the label of observations.
It was be shown like this picture.

<p align="center">
  <img src="https://raw.githubusercontent.com/peace195/peace195.github.io/master/images/rnn.jpg" alt="RNN"/>
</p>

How about choosing some distinct units inside the recurrent (LSTM, GRU, ...) layer of recurrent neural networks?
I have worked in some problem need to do that such as **sspect-based sentiment analysis**.
In this problem, we have to predict the polarity label of some aspect in a sentence or passage.
We usually use a tuple (positive, negative or neutral) for the polarity label.
E.g: 

* single aspect

{% highlight xml tabsize=4%}
<text>Love Al Di La</text>
<Opinions>
    <Opinion target="Al Di La" category="RESTAURANT#GENERAL" polarity="positive" from="5" to="13"/>
</Opinions>
{% endhighlight %}

* multiple aspect

{% highlight xml tabsize=4%}
<text>An awesome organic dog, and a conscious eco friendly establishment.</text>
<Opinions>
    <Opinion target="dog" category="FOOD#QUALITY" polarity="positive" from="19" to="22"/>
    <Opinion target="establishment" category="RESTAURANT#MISCELLANEOUS" polarity="positive" from="53" to="66"/>
</Opinions>
{% endhighlight %}

One of my proposal model is using bidirectional LSTM neural networks. It was be shown like picture below.
In this model, we only predict polarity label for the aspects in the sentence. But we have one problem is the different position of aspects in different sentence.
Uhmm, how will we solve it when we can not use the traditional RNN model?

 <img src="https://raw.githubusercontent.com/peace195/aspect-based-sentiment-analysis/master/model.png" alt="bilstm"/>

Here is the solution. We will use an additional **masking layer** after the LSTM layer.
It is a binary vector that ís 1.0 for aspect position, 0.0 for the others. We multiply LSTM layer with masking layer.
Then, we compute softmax, cross entropy and gradient descent as normally.

 <img src="https://raw.githubusercontent.com/peace195/peace195.github.io/master/images/mask.png" alt="bilstm"/>

Here is the [Tensorflow code example](https://github.com/peace195/aspect-based-sentiment-analysis/tree/master/code) for this solution:

{% highlight ruby tabsize=4%}
lstm_fw_cell = tf.nn.rnn_cell.BasicLSTMCell(nb_lstm_inside, forget_bias=1.0)
lstm_bw_cell = tf.nn.rnn_cell.BasicLSTMCell(nb_lstm_inside, forget_bias=1.0)

# pass lstm_fw_cell / lstm_bw_cell directly to tf.nn.bidrectional_rnn
# layers is an int number which is the number of RNNCell we would use
lstm_fw_multicell = tf.nn.rnn_cell.MultiRNNCell([lstm_fw_cell]*layers)
lstm_bw_multicell = tf.nn.rnn_cell.MultiRNNCell([lstm_bw_cell]*layers)

# get lstm cell output
outputs, _, _ = tf.contrib.rnn.static_bidirectional_rnn(lstm_fw_multicell,
                                                        lstm_bw_multicell,
                                                        X_train,
                                                        dtype='float32')

output_fw, output_bw = tf.split(outputs, [nb_lstm_inside, nb_lstm_inside], 2)
sentiment = tf.reshape(tf.add(output_fw, output_bw), [-1, nb_lstm_inside]) 

sentiment = tf.nn.dropout(sentiment, keep_prob)
sentiment = tf.add(tf.matmul(sentiment, sent_w), sent_b)
sentiment = tf.split(axis=0, num_or_size_splits=seq_max_len, value=sentiment)

# change back dimension to [batch_size, n_step, n_input]
sentiment = tf.stack(sentiment)
sentiment = tf.transpose(sentiment, [1, 0, 2])

# tf_X_binary_mask is the masking layer. 
sentiment = tf.multiply(sentiment, tf.expand_dims(tf_X_binary_mask, 2))

cross_entropy = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(logits=sentiment, labels=y_labels))
prediction = tf.argmax(tf.nn.softmax(sentiment), 2)
correct_prediction = tf.reduce_sum(tf.multiply(tf.cast(tf.equal(prediction, tf_y_train), tf.float32), tf_X_binary_mask))
{% endhighlight %}

**Does masking layer affect the gradient descent when training the model?**

#### Before masking
As far as we know, some of most important concepts:

* Recurrent Neural Networks

$$h_t = \sigma\big(W^{(hh)}h_{t-1} + W^{(hx)}x_{t}\big)$$

$$\hat{y}_t = softmax\big(W^{(S)}h_t\big)$$

* Cross Entropy Error

$$J^{(t)}(\theta) = -\sum_{j=1}^{3} y_{t,j}log\hat{y}_{t,j}$$

$$j$$ runs from 1 to 3 because there are three labels (positive, negative and neural) in our example problem.
* Mini-batched SGD

$$\theta^{new} = \theta^{old} - \alpha\nabla_{\theta}J_{t:t+B}(\theta)$$

$$B$$ is the batch size.
{: style="text-align: center"}

#### After masking
Consider that $$m$$ is the mask vector. Then, we have

$$h_t = \sigma\big(W^{(hh)}h_{t-1} + W^{(hx)}x_{t} \big)$$

$$l_{t} = h_t \cdot m_t$$

$$\hat{y}_t = softmax\big(W^{(S)}l_t\big)$$

If $$m_t = 0$$ then $$l_t = 1$$. Hence $$\hat{y}_t$$ depends on $$W^{(S)}$$ only.
Moreover, loss function $$J$$ depends on $$W^{(S)}$$ only too. It does not depend on the none-aspect word $$x_{[t]}$$.
Finally, the updating process using SGD depends on aspect word mainly because of its impact on softmax layer.
So we has very strong logic inside the model. That is: "All paremeter depend on the aspect mainly, especially for the last softmax layer using $$W^{(S)}$$ parameter".

Here are some pictures about batch train accuracy and loss when using masking for bidirectional LSTM model.

<p align="center">
  <img src="https://raw.githubusercontent.com/peace195/aspect-based-sentiment-analysis/master/code/accuracy_loss.png" alt="acc_loss"/>
</p>

