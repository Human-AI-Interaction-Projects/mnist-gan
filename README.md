# mnist-gan

UCLA ECE 209AS HCI Project, Fall 2020

![Video](https://github.com/binhanle/mnist-gan/blob/main/Video.mp4?raw=true)

## Authors

- Daniel Ahn
- Andrew Chen
- An Le

## Introduction

Our project expands on the paper by Bau et al. [1] that studies GAN dissections. The authors examined methods to forcibly change the outputs of Generative Adversarial Networks in order to better visualize and understand their internal representations. Our experiments attempt to answer an unsolved question from [1]: why does a GAN veto certain changes, but allow others? For example, to create an image of a church, a GAN will allow doors to be placed in realistic locations, such as on the wall of the building, but the network will not allow unrealistic placements, such as in the sky. 

To better understand this scenario, we chose to train a GAN on the MNIST dataset, where we can use each number as its own segmented feature. By dissecting our own GAN and altering internal feature maps, we are able to perform a series of experiments to determine what kinds of outputs we can forcibly create.

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

We implemented this GAN using PyTorch, and structured the generator as a list of separate layers. This made it easy to dissect the network and view feature maps while an output is produced.

## Feature Map Experiments

### Experiment 1

In this experiment, we swapped 25 random features, out of 128, of the source image with the donor image and saw that the Generator still creates an image that looks like the source image. This indicates that the Generator is vetoing these changes.
![experiment1a](https://github.com/binhanle/mnist-gan/blob/main/results/experiment1a.png?raw=true)

To see the extent to which random feature swaps would result in vetoing, we progressively increased the percentage of features swapped for several numbers.
![experiment1b_1](https://github.com/binhanle/mnist-gan/blob/main/results/experiment1b_1.png?raw=true)
![experiment1b_2](https://github.com/binhanle/mnist-gan/blob/main/results/experiment1b_2.png?raw=true)
![experiment1b_3](https://github.com/binhanle/mnist-gan/blob/main/results/experiment1b_3.png?raw=true)

### Experiment 2

Instead of randomly swapping feature maps, we attempted the most important ones. First, we wrote a function that determines the features that induce the most change in the source image according to the mean squared difference. Second, we utilized the function to morph a 1 into a 3 by swapping just 20 (16%) feature maps.

![experiment2a](https://github.com/binhanle/mnist-gan/blob/main/results/experiment2a.png?raw=true)

Finally, we repeated this process on another 1 and 3.

![experiment2b](https://github.com/binhanle/mnist-gan/blob/main/results/experiment2b.png?raw=true)

In both cases, the result more closely resembles a 3 than a 1, which suggests that samples of the same digits share the same important feature maps.

### Experiment 3

This time, we attempted to swap as many features as possible while maintaining the source image's shape. Instead of maximizing, we minimized the mean squared difference for every swap. We managed to swap 100, or 78% of the below 1's feature maps without significantly changing its form.

![experiment3](https://github.com/binhanle/mnist-gan/blob/main/results/experiment3.png?raw=true)

### Experiment 4

In this experiment, we returned to finding the most significant feature maps and investigated whether we could swap the same set of features on different samples of the same digits. Using only the 20 feature map indices obtained from Experiment 2, we successfully transformed different 1s into 3s.

![experiment4a](https://github.com/binhanle/mnist-gan/blob/main/results/experiment4a.png?raw=true)

However, applying the same swaps on a different digit pair, 0 and 4, does not yield satisfactory results. The results still resemble 0s, which demonstrates that different digit pairs have different representative features.

![experiment4b](https://github.com/binhanle/mnist-gan/blob/main/results/experiment4b.png?raw=true)

### Experiment 5
In this experiment, we want to investigate the effects of ablating filters that are important for generating a specific number. We can see the generate image and the result of a layer that contains 64 feature maps below. At this stage, some feature maps are already taking on the shape of the resulting image. To test the effects of filter ablation, we will ablate filters with a clear resemblance to the final image to see if that will allow our edits to propagate through.

![experiment5a_1](https://github.com/binhanle/mnist-gan/blob/main/results/experiment5a_1.png?raw=true)
![experiment5a_2](https://github.com/binhanle/mnist-gan/blob/main/results/experiment5a_2.png?raw=true)

Unfortunately, even with filter ablation, we were unable to propagate the results through to the final image. We believe the underlying cause of this issue is that the feature map representation of the Generator is too entangled.

![experiment5b_1](https://github.com/binhanle/mnist-gan/blob/main/results/experiment5b_1.png?raw=true)
![experiment5b_2](https://github.com/binhanle/mnist-gan/blob/main/results/experiment5b_2.png?raw=true)

## Resources
- [GAN Dissection: Visualizing and Understanding Generative Adversarial Networks](https://arxiv.org/abs/1811.10597) (Bau et al., 2019) - Paper that motivated this project
- [lyeoni/pytorch-mnist-GAN](https://github.com/lyeoni/pytorch-mnist-GAN) - GitHub project from which we adapted the GAN training routine
- [PyTorch DCGAN Tutorial](https://pytorch.org/tutorials/beginner/dcgan_faces_tutorial.html) - Our architecture is loosely based on the one described in this tutorial
