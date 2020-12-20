# mnist-gan

## Introduction

## GAN Architecture

### Generator

| Layer | Kernel size | Output shape |
| --- | --- | --- |
| Input | - | 5 |
| FC1 | - | 256 &times; 7 &times; 7 |
| Conv2 | 5 &times; 5 | 128 &times; 7 &times; 7 |
| Conv3 | 5 &times; 5 | 64 &times; 14 &times; 14 |
| Conv4 | 5 &times; 5 | 1 &times; 28 &times; 28 |

### Discriminator

| Layer | Kernel size | Output shape |
| --- | --- | --- |
| Input | - | 1 &times; 28 &times; 28 |
| Conv1 | 5 &times; 5 | 64 &times; 14 &times; 14 |
| Conv2 | 5 &times; 5 | 128 &times; 7 &times; 7 |
| FC3 | - | 1 |

## GAN Implementation

## Feature Map Experiments

### Experiment 1

![experiment1a](https://github.com/binhanle/mnist-gan/blob/main/results/experiment1a.png?raw=true)
![experiment1b_1](https://github.com/binhanle/mnist-gan/blob/main/results/experiment1b_1.png?raw=true)
![experiment1b_2](https://github.com/binhanle/mnist-gan/blob/main/results/experiment1b_2.png?raw=true)
![experiment1b_3](https://github.com/binhanle/mnist-gan/blob/main/results/experiment1b_3.png?raw=true)

### Experiment 2

### Experiment 3

### Experiment 4

### Experiment 5
![experiment5a_1](https://github.com/binhanle/mnist-gan/blob/main/results/experiment5a_1.png?raw=true)
![experiment5a_2](https://github.com/binhanle/mnist-gan/blob/main/results/experiment5a_2.png?raw=true)

![experiment5b_1](https://github.com/binhanle/mnist-gan/blob/main/results/experiment5b_1.png?raw=true)
![experiment5b_2](https://github.com/binhanle/mnist-gan/blob/main/results/experiment5b_2.png?raw=true)

## Resources
- [GAN Dissection: Visualizing and Understanding Generative Adversarial Networks](https://arxiv.org/abs/1811.10597) Inspiration for this project
- [lyeoni/pytorch-mnist-GAN](https://github.com/lyeoni/pytorch-mnist-GAN) Project from which we adapted the GAN training routine
- [PyTorch DCGAN Tutorial](https://pytorch.org/tutorials/beginner/dcgan_faces_tutorial.html) Our architecture is loosely based on the one described in this tutorial
