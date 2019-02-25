# Backpropagation

## Overview

The purpose of this lab is to give you a chance to practice the details of the backpropogation algorithm.

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
