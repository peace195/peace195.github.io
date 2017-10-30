---
title: Convert a string representation of list to list in Python
---

Preprocess data is a big step when you do a machine learning problem.
In this post, I will show a trick to import text data such as word embeddings, a dictionary of lists, which usually has a format likes:

{% highlight plain-text %}
cat: [1, 1.2, 3, 4, 6.2]
dog: [1.2, 1, 3.1, 4, 6]
bird: [2, 4, 5.2, 6, 8]
{% endhighlight %}

How can we load the string representation of list in each line as a real list? I knew two ways to do it.

* The way, that I used to do, is using **regex expression** to find the number in numerical part of data. 

```python
import re
from collections import defaultdict

d_list = defaultdict(list)

f = open('data.txt', 'r')
for line if f:
    parts = line.split(':')
    d_list[parts[0].strip()] = map(float, re.findall("[-+]?\d+[\.]?\d*[eE]?[-+]?\d*", parts[1]))
f.close()
```

* A simpler way is using **ast library**.

{% highlight python tabsize=4%}
import ast
from collections import defaultdict

d_list = defaultdict(list)

f = open('data.txt', 'r')
for line if f:
    parts = line.split(':')
    d_list[parts[0].strip()] = ast.literal_eval(parts[1].strip())
f.close()
{% endhighlight %}

* Another example of mine: [https://github.com/peace195/LateFusion/blob/master/cmc_curve.ipynb](https://github.com/peace195/LateFusion/blob/master/cmc_curve.ipynb)