---
title: Some Deep Learning late fusion techniques based on the score of observations
---

"Many heads are better than one". It is how fusion works. There are early fusion, middle fusion, and late fusion techniques. In this post, I focused on some late fusion techniques based on the score of observations.

## 1. Late fusion techniques
### Transformation-based approaches
Let I define that $$f(x)$$ is the vector score of observation $$x$$ through a prediction model such as softmax layer in Deep Learning. So that $$i = argmax(f(x))$$ is the class that observation belong to.

Max rule:

$$f(x, y) = max(f(x), f(y))$$
 
Sum rule:

$$f(x, y) = f(x) + f(y)$$
 
Product rule is based on the assumption of statistical independence of the representations.
 
$$f(x, y) = f(x) \cdot f(y)$$

### Classification-based approaches
We map $$(f(x_i), f(y_i))$$ is a sample for species $$i$$ in Euclid space. If the ground-truth species of the training images and the predicted class are identical, it is a positive sample. On the other hand, if the actual class of images is different from
the predicted class, it is defined as a negative sample. We train an SVM classifier using positive and negative samples. In the test phase, the
distance to the decision bound is the measure to decide the labels of testing samples.

$$f(x, y) = SVM(f(x), f(y))$$

### My proposal hybrid model

$$f(x, y) = SVM(f(x), f(y)) \cdot f(x) \cdot f(y)$$

## 2. Experiments
R1 and R5 are rank 1 and rank 5 accuracy.

### Combinations of multi Deep Learning models
Dataset: Used 50 species from [flower dataset](http://www.imageclef.org/lifeclef/2015/plant) in PlantClef2015 competition.

Result: Combined two popular CNN models which are AlexNet and GoogleNet.

{% highlight plain-text %}
| Accuracy (%) | AlexNet | GoogleNet | Max  | Sum  | Product | SVM  | Hybrid |
| ------------ | ------- | --------- | ---- | ---- | ------- | ---- | ------ |
|       R1     |  73.0   |   74.0    | 77.0 | 77.0 |   78.0  | 78.0 |  78.2  |
|       R5     |  90.8   |   91.2    | 93.0 | 93.0 |   93.0  | 92.0 |  93.0  |
{% endhighlight %}

### Combinations of multi observations
Dataset: Used 50 species from [flower, leaf, entire, branch dataset](http://www.imageclef.org/lifeclef/2015/plant) in PlantCLEF2015 competition.

Single organ

{% highlight plain-text %}
|  Organ     | R1 (%) | R5 (%) |
| ---------- | ------ | ------ |
| Leaf (Le)  |  66.2  |  89.8  |
| Flower (Fl)|  73.0  |  90.8  |
| Branch (Br)|  43.2  |  70.4  |
| Entire (En)|  32.4  |  64.0  |
{% endhighlight %}

Combined pairs of organ based on score of AlexNet

{% highlight plain-text %}
|        | Accuracy (%) |  Max  | Sum  | Product  | SVM  | Hybrid |
| ------ | ------------ | ----- | ---- | -------- | ---- | ------ |
|En - Le |      R1      |  66.2 | 67.2 |   75.6   | 74.0 |   76.6 |
|        |      R5      |  88.6 | 88.8 |   93.2   | 81.8 |   94.6 |
|En - Fl |      R1      |  73.8 | 74.4 |   78.8   | 77.2 |   81.2 |
|        |      R5      |  92.6 | 92.8 |   94.2   | 84.2 |   94.4 |
|Le - Fl |      R1      |  81.6 | 82.0 |   88.6   | 86.2 |   89.8 |
|        |      R5      |  96.8 | 96.8 |   98.2   | 90.4 |   98.4 |
|Br - Le |      R1      |  70.2 | 71.0 |   76.8   | 73.8 |   78.4 |
|        |      R5      |  89.6 | 90.0 |   93.4   | 79.6 |   93.8 |
|Br - Fl |      R1      |  74.2 | 75.4 |   80.8   | 79.0 |   81.4 |
|        |      R5      |  90.8 | 91.4 |   95.2   | 83.0 |   95.4 |
|Br - En |      R1      |  51.6 | 52.2 |   58.0   | 58.0 |   58.6 |
|        |      R5      |  76.8 | 77.6 |   83.6   | 81.4 |   83.8 |
{% endhighlight %}

## 3. Conclusions
The experiments show that the fusion techniques increase the plant identification accuracy. Also, my hybrid model presents the best results in all evaluation.