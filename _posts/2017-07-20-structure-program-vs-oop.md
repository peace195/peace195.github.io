---
title: Structure programming versus Object-oriented programming in Deep Learning
---

Structured programming (SP) is a subset of object-oriented programming (OOP).
Therefore, OOP can help in developing much larger and complex programs than structured programming.
Traditional procedural programming is far less dogmatic. You have data. You have functions. You apply the functions to the data.
If you want to organize your program somehow, that's your problem, and the language really isn't going to help you.
In my working experiences, if you want to try some models quickly, you could use SP step-by-step.
But when working with a system with multiple modules, you should use OOP for Deep Learning because of its encapsulation, inheritance property.
Besides that, OOP make your Deep Learning program more brightly and concisely.

* Structured programming template

{% highlight ruby tabsize=4%}
def load_data(data_dir):
    '''
    Preprocess data for our model.
    Return some useful data such as train and test data,
    word2vec matrix, mask, pretrained parameters, etc ...
    '''
    return x_train, y_train, x_test, y_test, ...

def model(parameters, x_train, y_train, x_test, y_test, ...):
    graph = tf.Graph()
    with graph.as_default(), tf.device('/cpu:0'):
        # build your graph model here
    
    with tf.Session(graph = graph) as sess:
        init = tf.global_variables_initializer()
        sess.run(init)
        # training and testing
    return
        
def main():
    # load_data(...)
    # model(...)
    return
    
if __main__ == '__main__':
    main()
{% endhighlight %}

When our system is big and complex, it is not convenience for passing data and parameters to modeling.
Otherwises, It is hard for using the model for some backend services. I proposed a solution using OOP below.

* Object-oriented programming

{% highlight ruby tabsize=4%}
class Data:
    '''
    This class contains train, test, valid data or word2vec, word dict, etc, ...
    '''
	
    def __init__(self, data_dir, ...):
        self.data_dir = data_dir
        
    def load_data(self):
        return
        
class Model:
    def __init__(self, parameters, sess):
        self.sess = sess
        
    def modeling(self):
        return
		
    def load_model(self):
        return
		
    def save_model(self):
        return
		
    def predict(self, data):
        return
		
    def evaluate(self, data):
        return
		
    def train(self, data):
        return
		
def main():
    sess = tf.Session()
    data = Data(data_dir, ...)
    model = Model(parameters, sess)
    
    model.train(data)
    # model.evalate(data)
    # model.predict(data)
    
if __main__ == '__main__':
    main()
{% endhighlight %}
