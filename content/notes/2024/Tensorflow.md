
+++
title = 'Tensorflow'
date = 2024-06-08T13:42:15.1515+05:30
draft = true
tags =[]
+++ 



## Tensors
- Scalar (Rank-0)
- Vector (Rank-1)
- Matrix (Rank-2)
- Higher Rank 

Properties of tensors
- shape
- Data type 
- vaules

Operations

```python
tensor_a= tf.constant([[1,3],[3,4]])
tensor_b= tf.constant([[1,3],[3,4]])

tf.add(tensor_a,tensor_b)
tf.sub(tensor_a,tensor_b)
tf.mul(tensor_a,tensor_b)
# matrix mul
tf.matmul(tensor_a,tensor_b)

```


### constant and variables

In TensorFlow, constants are values that don't change during the execution of the program. They are useful for creating nodes in the computational graph with fixed values. Constants are created using `tf.constant`.

Variables, on the other hand, maintain state across sessions and are used for parameters in machine learning models. They can be updated during the training process. Variables are created using `tf.Variable`

```python
import tensorflow as tf

# Define a constant
a = tf.constant(5, dtype=tf.int32)
b = tf.constant(6, dtype=tf.int32)

# Perform an operation
c = tf.add(a, b)

# Initialize a session to run the graph
with tf.Session() as sess:
    result = sess.run(c)
    print(result)  # Output: 11



# Define a variable
weight = tf.Variable(0.5, dtype=tf.float32)

# Perform an operation
new_weight = weight * 2

# Initialize all variables
init = tf.global_variables_initializer()

# Initialize a session to run the graph
with tf.Session() as sess:
    sess.run(init)
    result = sess.run(new_weight)
    print(result)  # Output: 1.0

```

### Graphs and sessions
A computational graph is a series of TensorFlow operations arranged into a graph of nodes. Each node represents an operation, and edges represent the data (tensors) flowing between these operations.

### Sessions

A session is used to execute the operations defined in the computational graph. It allocates resources (such as GPU memory) and manages the execution of operations.

```python
import tensorflow as tf

# Define a simple computational graph
a = tf.constant(5)
b = tf.constant(3)
c = tf.add(a, b)

# Create and run a session to execute the graph
with tf.Session() as sess:
    result = sess.run(c)
    print(result)  # Output: 8

```

- **Graph**: The blueprint of operations and data flow.
- **Session**: The runtime environment for executing the graph.

### Graphs and Sessions in TensorFlow 2.x

In TensorFlow 2.x, eager execution is enabled by default, making the framework more intuitive and user-friendly. Operations are executed immediately as they are called from Python. However, you can still create graphs and sessions using `tf.function` for more complex or performance-critical scenarios.

```python
import tensorflow as tf

# Define a function that builds a graph
@tf.function
def compute(a, b):
    return tf.add(a, b)

# Call the function
result = compute(5, 3)
print(result)  # Output: 8

```

### Transitioning from TensorFlow 1.x to 2.x

- In TensorFlow 1.x, you explicitly define the graph and then create a session to execute it.
- In TensorFlow 2.x, eager execution is the default mode, and you can use `tf.function` to create a graph if needed.

### Tensorboard

TensorBoard is a suite of visualization tools provided by TensorFlow that enables you to inspect and understand your machine learning workflows.

### Visualizing the computational graph


```python
# below is version 1
import tensorflow as tf

# Reset the default graph
tf.reset_default_graph()

# Define the computational graph
a = tf.constant(5, name='a')
b = tf.constant(3, name='b')
c = tf.add(a, b, name='c')

# Create a summary to visualize in TensorBoard
writer = tf.summary.FileWriter('./logs', tf.get_default_graph())

with tf.Session() as sess:
    result = sess.run(c)
    print(result)  # Output: 8

# Close the writer
writer.close()

# below is version 2

import tensorflow as tf

# Define the function
@tf.function
def my_func(a, b):
    return tf.add(a, b, name='c')

# Create a summary writer
logdir = './logs'
writer = tf.summary.create_file_writer(logdir)

# Trace the function and log the graph
tf.summary.trace_on(graph=True, profiler=True)

result = my_func(tf.constant(5), tf.constant(3))

with writer.as_default():
    tf.summary.trace_export(name="my_func_trace", step=0, profiler_outdir=logdir)

```

```
tensorboard --logdir=./logs
```




## Pytroch 

http://blog.ezyang.com/2019/05/pytorch-internals/ 