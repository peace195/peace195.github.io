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

![Spatial Pyramid Pooling](https://raw.githubusercontent.com/peace195/peace195.github.io/master/images/SppNet.png)

## Inside the ideal


## Tensorflow porting
You can see the full code and a SPP on top of Alexnet example [here](https://github.com/peace195/sppnet).
## Up-downside


## Conclusions
I have just analysis some idea of SPP. SPP is a beautiful idea that combine classic computer visions idea to modern neural network. Pheww, hope you like it. :D

## References
[1] [Spatial Pyramid Pooling in Deep Convolutional Networks for Visual Recognition](https://arxiv.org/abs/1406.4729)
