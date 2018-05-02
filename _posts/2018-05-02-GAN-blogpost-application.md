# Introduction
In previous lessons, we have seen different types of neural networks that solve **classification problems.** Convolutional neural networks are well-suited for spatial data, making them a good choice for image classification. Recurrent neural networks are well-suited for sequential data, and thus are good for classifying English sentences as gramatically correct or incorrect. As we will see, generative adversarial networks (GANs) utilize these classification capabilities in a clever way to extend the applications of machine learning.


Specifically, GANs have the ability to generate new data, for example allowing us to generate images of dogs. Going a step further, it is even possible to specify what type of dog: fur color, size, perpective angle, etc. More formally, we have seen how traditional neural networks take in high-dimensional input data **X** and return a low-dimensional output **Y**, such as a number in the range **[0,1]**. GANs do the opposite: given **Y**, its goal is to output any possible **X** such that, when fed into a traditional neural net, it would classify it with the value **Y**. 


# Generator vs. Discriminator

GANs are comprised of two major components, a **generator** and a **discriminator**. These are both neural networks, and they work together to give the overall functionality of the GAN. The discriminator is nothing new, it is a traditional neural network that classifies its input by outputting a number. The generator, however, can be thought of as the "reverse" of a traditional neural network. It takes a low-dimensional input, often just a random noise distribution vector, and produces a high-dimensional output, such as an image of what it believes to be a dog. To get a sense of how these pieces fit together, look at the image below:

![missing diagram of generator/discriminator]()



# Training


# Questions


# Answers & Explanations


# Resources
https://deeplearning4j.org/generative-adversarial-network


# References
https://hackernoon.com/generative-adversarial-networks-a-deep-learning-architecture-4253b6d12347
