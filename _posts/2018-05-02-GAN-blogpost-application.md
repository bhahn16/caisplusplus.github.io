# Introduction
In previous lessons, we have seen different types of neural networks that solve **classification problems.** Convolutional neural networks are well-suited for spatial data, making them a good choice for image classification. Recurrent neural networks are well-suited for sequential data, and thus are good for classifying English sentences as gramatically correct or incorrect. As we will see, generative adversarial networks (GANs) utilize these classification capabilities in a clever way to extend the applications of machine learning.


Specifically, GANs have the ability to generate new data, for example allowing us to generate images of dogs. Going a step further, it is even possible to specify what type of dog: fur color, size, perpective angle, etc. More formally, we have seen how traditional neural networks take in high-dimensional input data **X** and return a low-dimensional output **Y**, such as a number in the range **[0,1]**. GANs do the opposite: given **Y**, its goal is to output any possible **X** such that, when fed into a traditional neural net, it would classify it with the value **Y**. 


# Generator vs. Discriminator

GANs are comprised of two major components, a **generator** and a **discriminator**. These are both neural networks, and they work together to give the overall functionality of the GAN. The discriminator is nothing new, it is a traditional neural network that classifies its input by outputting a number. The generator, however, can be thought of as the "reverse" of a traditional neural network. It takes a low-dimensional input, often just a random noise distribution vector, and produces a high-dimensional output, such as an image of what it believes to be a dog. To get a sense of how these pieces fit together, look at the image below:

![missing diagram of generator/discriminator](https://github.com/bhahn16/caisplusplus.github.io/blob/master/images/GAN_diagram.png)

The generator and discriminator are adversaries. In other words, they are trying to fool each other. The generator wants to maximize the error function of the discriminator, while the discriminator wants to minimize its own error function. Notice how the discriminator is fed real images from the data set, as well as artificial images piped in from the generator. The discriminator must determine if its input is real or fake, and the generator must make its output realistic enough to fool the discriminator. As the discriminator gets better, the generator must improve to keep up, and vice versa. The result after enough training is a powerful generator neural network that is capable of generating realistic images!

There is a common analogy used to help grasp this concept. Think of the discriminator as the police and the generator as a criminal who makes fake ID's. At first, the criminal is not very smart, and the ID's he produces are clearly fake (like [this one](https://images-na.ssl-images-amazon.com/images/I/51kGMAUsTSL._SY300_QL70_.jpg)). 



# Training


# Questions


# Answers & Explanations


# Resources
https://deeplearning4j.org/generative-adversarial-network


# References
https://hackernoon.com/generative-adversarial-networks-a-deep-learning-architecture-4253b6d12347
