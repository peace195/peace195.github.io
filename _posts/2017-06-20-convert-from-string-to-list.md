---
title: Convert string representation of list to list in Python
---

I worked with a lot of data. Sometimes, I have to load data from file with form

{% highlight plain-text %}
cat: [1, 1.2, 3, 4, 6.2]
dog: [1.2, 1, 3.1, 4, 6]
bird: [2, 4, 5.2, 6, 8]
{% endhighlight %}

How can we load the string representation of list in each line as a real list? I known two ways to do it.

* The way, that I used to do, is using **regex expression** to find the number in numerical part of data. 

{% highlight python tabsize=4%}
import re
from collections import defaultdict

d_list = defaultdict(list)

f = open('data.txt', 'r')
for line if f:
	parts = line.split(':')
	d_list[parts[0].strip()] = map(float, re.findall("[-+]?\d+[\.]?\d*[eE]?[-+]?\d*", parts[1]))
f.close()
{% endhighlight %}

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