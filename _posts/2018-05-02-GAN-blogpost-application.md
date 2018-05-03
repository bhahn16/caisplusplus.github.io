# Introduction
In previous lessons, we have seen different types of neural networks that solve **classification problems.** Convolutional neural networks are well-suited for spatial data, making them a good choice for image classification. Recurrent neural networks are well-suited for sequential data, and thus are good for classifying English sentences as gramatically correct or incorrect. As we will see, generative adversarial networks (GANs) utilize these classification capabilities in a clever way to extend the applications of machine learning.


Specifically, GANs have the ability to generate new data, for example allowing us to generate images of dogs. Going a step further, it is even possible to specify what type of dog: fur color, size, perpective angle, etc. More formally, we have seen how traditional neural networks take in high-dimensional input data **X** and return a low-dimensional output **Y**, such as a number in the range **[0,1]**. GANs do the opposite: given **Y**, its goal is to output any possible **X** such that, when fed into a traditional neural net, it would classify it with the value **Y**. 


# Generator vs. Discriminator

GANs are comprised of two major components, a **generator** and a **discriminator**. These are both neural networks, and they work together to give the overall functionality of the GAN. The discriminator is nothing new, it is a traditional neural network that classifies its input by outputting a number. The generator, however, can be thought of as the "reverse" of a traditional neural network. It takes a low-dimensional input, often just a random noise distribution vector, and produces a high-dimensional output, such as an image of what it believes to be a dog. To get a sense of how these pieces fit together, look at the image below:

![missing diagram of generator/discriminator](https://github.com/bhahn16/caisplusplus.github.io/blob/master/images/GAN_diagram.png)

The generator and discriminator are adversaries. In other words, they are trying to fool each other. The generator wants to maximize the error function of the discriminator, while the discriminator wants to minimize its own error function. Notice how the discriminator is fed real images from the data set, as well as artificial images piped in from the generator. The discriminator must determine if its input is real or fake, and the generator must make its output realistic enough to fool the discriminator. As the discriminator gets better, the generator must improve to keep up, and vice versa. The result after enough training is a powerful generator neural network that is capable of generating realistic images!

There is a common analogy used to help grasp this concept. Think of the discriminator as the police and the generator as a criminal who makes fake ID's. At first, the criminal is not very smart, and the ID's he produces are clearly fake (like [this one](https://images-na.ssl-images-amazon.com/images/I/51kGMAUsTSL._SY300_QL70_.jpg)). But after being caught by the police a few times, the criminal learns how to make the ID's more realistic. Eventually he is able to fool the police. But then, the police adapt more sophisticated methods to catch fake ID's, and the criminal must find a way to improve his ID's even more. It is an ongoing battle between two adversaries: the police and the criminal, each getting better as the other improves.


**Checkpoint Question 1:** Which of the following statement(s) are true regarding the input for the generator?

I) The input vector can contain any arbitray noise

II) The input vector must have the same dimensionality as the output

III) The input vector must remain constant throughout training

(See answer at the bottom)


# Training

The generator and discriminator start off very unintelligent, and must improve by battling each other. Here is the high-level list of steps that occur during training of a generative network, using the example of creating realistic images of dogs.

1. Input a noise vector into the generator and produce **N** fake images. Note they will not be very good.
2. Acquire a sample of **N** dog images from the real dataset.
3. Train the discriminator with both of these image sets.
4. Now the discriminator is somewhat smart, so to improve the generator, first "combine" the networks by piping the generator output into the discriminator.
5. Train this one large network, with the fake images labeled as real. The goal is for all fake images to be reported as real by the discriminator. **Note:** freeze the weights of the discriminator to prevent this process from confusing/manipulating the discriminator, since we are focused on the generator now.
6. Repeat all of these steps until the generator produces images of dogs that look realistic to a human.


Training a discriminator is very similar to what we have seen before. We want it to minimize its error function using gradient descent, and determine its parameter weights through backpropogation. This will almost always mean using a sigmoid error function, where the error for one input is calculated as:

![missing image](https://github.com/bhahn16/caisplusplus.github.io/blob/master/images/GAN_errornosum.png)

And so for all inputs **N**, 

![missing image](https://github.com/bhahn16/caisplusplus.github.io/blob/master/images/GAN_errorsum2.png)

As for the generator, there are a few options for the error function. One option is to use the same error function, but instead try to maximize it using gradient "ascent". While this can work, it is not always recommended because if the discriminator becomes too good at predicting whether images are real or fake (i.e. returning numbers very close to 0 and 1) then the gradient for the generator will be very small. It will then be extremely hard for the generator to know which way to change its parameters and improve because of the **"vanishing gradient"** issue. However, manipulating the learning rates of each side can help alleviate this problem. Luckily there are two other closed-form error functions described [here](https://danieltakeshi.github.io/2017/03/05/understanding-generative-adversarial-networks/), with a drawback being they are a little beyond the introductory level and will not be elaborated on here.


### Controlling the Output

As mentioned in the introduction, in the case of generating pictures of dogs, it is possible to specify certain attributes of the output, such as fur color, size, and more. Below is an example from a study that showed how to generate a human face, specifying that it must be the face of a woman wearing sunglasses.

![missing image](https://github.com/bhahn16/caisplusplus.github.io/blob/master/images/GAN_glasses.jpeg)


First, imagine all image vectors represents a point in a high-dimensional space. This is often referred to as the **latent space**, and it turns out images that are generally similar have points in the latent space relatively close to each other. There are continuous paths through this space, which can be traversed by changing the values of the image vector components. This means it is possible to perform image addition and subtraction to construct new images. In other words, let's say I'm given the latent vectors of the face of a man **M**, the face of a man with sunglasses **G**, and the face of a woman **W**. Using vector addition and subtraction, the following expression would evaluate to an image vector of a woman wearing sunglasses: **G** - **M** + **W**.

Let's apply this to GANs by doing the same thing, using the example above. First use the generator to create a bunch of pictures of men with glasses, and average all of the *input* vectors used to generate these images. Do the same for pictures of men without glasses, and women without glasses. Call these averaged vectors **G'**, **M'** and **W'** respectively. Perform the same vector operation **G'** - **M'** + **W'** to get an input vector that, when passed through the generator, will produce a woman wearing sunglasses. This works because all we are doing is moving through the latent space.

# Review Questions


# Answers & Explanations

**Checkpoint Question 1: I only**

I is true because the generator will manipulate whatever data it is given into the appropriate output. It doesn't matter if the fibonacci sequence or random numbers are fed in, they both carry no meaning to a machine. It is simply a starting point.

II is false because the generator can upscale low-dimensional input into high-dimensional input. Given random input with dimensionality 10, there are many different ways to scale it up to a 784-dimensional image. Note that the input dimensionality cannot be greater than the output dimensionality.

III is false because all the generator needs to do is create realistic images to fool the discriminator. It's not necessary to hold the input vector constant, if anything doing so may be bad because it could make the generator's output more predictable.

# Resources
https://deeplearning4j.org/generative-adversarial-network


# References
https://hackernoon.com/generative-adversarial-networks-a-deep-learning-architecture-4253b6d12347
