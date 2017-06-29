---
title: Upside and downside of spatial pyramid pooling
---

## Abstracts

Spatial Pyramid Pooling[1] (SPP) is a great idea that do not need resize image before feeding to the neural network.
In other words, it uses multi-level pooling to adapts multiple image's size and keep the original features of them.
SPP is inspired from:

* [Pyramid (image processing)](https://en.wikipedia.org/wiki/Pyramid_(image_processing))
* [Bag-of-words](https://en.wikipedia.org/wiki/Bag-of-words_model_in_computer_vision)

In this post, I am going to show mathematic inside before porting it into tensorflow version and analysing upside and downside of it.

<p align="center">
  <img src="https://raw.githubusercontent.com/peace195/peace195.github.io/master/images/SppNet.png" alt="Spatial Pyramid Pooling"/>
</p>

## Inside the ideal


## Tensorflow porting
{% highlight python tabsize=4%}
def spatial_pyramid_pool(previous_conv, num_sample, image_size, out_pool_size):
    '''
	previous_conv: a tensor vector of previous convolution layer
	num_sample: an int number of image in the batch
	image_size: an int vector [height, width] is the image size of all image in the batch
	out_pool_size: a int vector of expected output size of max pooling layer
	
	returns: a tensor vector with shape [1 x n] is the concentration of multi-level pooling
	'''
        
    spp = tf.Variable(tf.truncated_normal([num_sample, ] stddev=0.01))
    
    for i in range(0, len(out_pool_size)):
        h_strd = image_size[0] / out_pool_size[i]
        w_strd = image_size[1] / out_pool_size[i]
        h_wid = image_size[0] - h_strd * out_pool_size[i] + 1
        w_wid = image_size[1] - w_strd * out_pool_size[i] + 1
        max_pool = tf.nn.max_pool(previous_conv,
                                   ksize=[1,h_wid,w_wid, 1],
                                   strides=[1,h_strd, w_strd,1],
                                   padding='VALID')
        if (i == 0):
			spp = tf.reshape(max_pool, [num_sample, -1])
		else:
			spp = tf.concat(1, [spp, tf.reshape(max_pool, [num_sample, -1])])
    
    return spp
{% endhighlight %}

You can see the full code and a SPP on top of Alexnet example [here](https://github.com/peace195/sppnet).
## Up-downside


## Conclusions
I have just analysis some idea of SPP. SPP is a beautiful idea that combine classic computer visions idea to modern neural network. Pheww, hope you like it. :D

## References
[1] [Spatial Pyramid Pooling in Deep Convolutional Networks for Visual Recognition](https://arxiv.org/abs/1406.4729)
