# Backpropagation

## Overview

The purpose of this lab is to give you a chance to practice the details of the backpropogation algorithm.

You'll start by working through the calculations by hand, then move to a Python implementation.

## Network

Consider a network with two inputs, *x<sub>1</sub>* and *x<sub>2</sub>*, two hidden neurons and one output neuron. The network has 9
total parameters:

- *w<sub>11</sub>*, the weight connecting input 1 to hidden neuron 1
- *w<sub>12</sub>*, the weight connecting input 2 to hidden neuron 1
- *w<sub>10</sub>*, the bias for hidden neuron 1, which has an implicit associated input of *x<sub>0</sub>* = 1

- *w<sub>21</sub>*, the weight connecting input 1 to hidden neuron 2
- *w<sub>22</sub>*, the weight connecting input 2 to hidden neuron 2
- *w<sub>20</sub>*, the bias for hidden neuron 1, which has an implicit associated input of *x<sub>0</sub>* = 2

- *w<sub>31</sub>*, the weight connecting hidden-layer neuron 1 to the output neuron
- *w<sub>32</sub>*, the weight connecting hidden-layer neuron 2 to the output neuron
- *w<sub>30</sub>*, the bias for the output neuron, which has an implicit associated input of *o<sub>0</sub>* = 1

## Diagram

Draw a diagram of network including all of the connections (make it big), and label everything with its appropriate weight.

## Forward-Calculation

Let's start with the following weights:

- *w<sub>11</sub>* = 1
- *w<sub>12</sub>* = 1
- *w<sub>10</sub>* = 0

- *w<sub>21</sub>* = 1
- *w<sub>22</sub>* = 1
- *w<sub>20</sub>* = 0

- *w<sub>31</sub>* = 1
- *w<sub>32</sub>* = 1
- *w<sub>30</sub>* = 0

(All of the connections are 1 and all of the biases are zero).

We'll work on training the logical-AND function that we previously used for the single layer perceptron. Here is its truth table:

```
x_1    x_2  | target label (t)
------------------------------
 0      0            0
 0      1            0
 1      0            0
 1      1            1
```

First, use the network as configured to attempt to classify each point. Perform a forward-pass through the network by hand for each of
the four points. What results do you obtain?

```
for each hidden layer node j:
    y_j = the weighted sum of inputs to node j
    o_j = sigmoid(y_j)

for the output layer node k:
    y_k = the weighted sum of hidden layer inputs to node k
    o_k = sigmoid(y_k)

if o_k <= .5:
    assign class 0
else:
    assign class 1
```

## Train

We'll now train the network by hand using the backpropagation update rules. I'm not giving the update rules here, because they're hard to type.

Iterate one point at a time. Use a **learning rate of .1**.

- Classify the point using the forward pass. In doing so, you'll obtain values for each *y<sub>j</dub>* and *o<sub>j</dub>* at the hidden-layer nodes, and *y<sub>k</dub>* and *o<sub>k</dub>* at the output layer.

- Update the weights (and the bias) at the output node using its update rule.

- Update the weights and bias at each of the two hidden-layer nodes using their update rules.

- The updates take place even if the assigned class matches the target class: use the value of the output sigmoid function to calculate the error

Do one epoch, training over all four points in the logical-AND function, updating the weights after each point.

After you update the weights, verify that they have moved in a direction that makes the classification **more correct**, even if the output has not crossed the threshold of .5 to actually change its predicted class.
