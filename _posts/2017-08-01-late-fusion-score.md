---
layout: post
title: Some late fusion techniques based on the score of observations
---

"Many heads are better than one". This is how fusion works. There are early fusion, middle fusion and late fusion techniques.
In this post, I focused to  some late fusion techniques based on the score of observations.

### Transformation-based approaches

Let I define that $$f(x)$$ is the vector score of observation $$x$$ through a prediction model such as softmax layer in Deep Learning.
So that $$i = argmax(f(x))$$ is the class that observation belong to.
Max rule is one of the most common transformation-based approaches. Maximal score is selected as a single fused score:

$$f(x, y) = max(f(x), f(y))$$
 
Sum rule is also the representative of the transformationbased approaches. Summation of the multiple scores provides a single fused score. It is defined by:

$$f(x, y) = f(x) + f(y)$$
 
Product rule is based on the assumption of statistical independence of the representations.
This assumption is reasonable because observations of a certain class are mutually independent.
 
$$f(x, y) = f(x) \cdot f(y)$$

### Classification-based approaches

Once the ground-truth species of the training images and the predicted
class are the identical, they are the positive samples. On
the other hand, the actual class of images is different from
the predict class. They are defined as negative samples. A SVM
classifier is learnt by using positive and negative
training samples in the score space. In the test phase, the
distance to the decision bound is the measure to decide the
labels of testing samples.

$$f(x, y) = SVM(f(x), f(y))$$

### My proposal hybrid model

$$f(x, y) = SVM(f(x), f(y)) \cdot f(x) \cdot f(y)$$

