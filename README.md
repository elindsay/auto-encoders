# Experiments with Variational Autoencoders
I had been generating small video clips with Runway ML, which generates 4-16 second video clips. I wanted to be able to connect different clips together using a smooth transition, as opposed to an abrupt cut. Most of the videos were fairly psychedelic in nature, so it wasn’t really important to create something “normal” looking, but more of like, a brief surrealist transition 

The issue, is that while many of the generative AI tools I was using on the internet could “imagine” images from scratch, they couldn’t interpolate well between two images. I’d recently learned about variational autoencoders, and was fascinated by the idea of “encoding” an image into a “latent space”. I thought that, using a (probably linear) interpolation between two encoded images in the latent space, it would be possible to create a continuous (if possibly slightly surreal) transition between two images. 


## 1 - MNIST Variational Autoencoder
The goal of this project, was to see if I was correct — that a linear interpolation between the encoded values of two images would produce a smooth transition when those values were decoded. (Note: I actually attempted this first using a simple autoencoder, not a variational autoencoder, and realized that the transition simply looked like a crossfade, which was not interesting to me given that you could easily generate such an effect in a video editing program.)

Code for this project: https://colab.research.google.com/drive/1-03eB9covktokFlcFBpvE-LnXg-invpw

Most of this code is very similar to the variational autoencoder code from the MNIST lab of the”Generative Deep Learning with Tensorflow” lab in Coursera (https://www.coursera.org/learn/generative-deep-learning-with-tensorflow) the main difference being that it uses convolutional layers for the autoencoder. I kept the latent space two dimensional, like it had been in the original lab, because it seemed to give adequate results for my purposes, and it would be easier for me to debug than a higher dimensional space. 

### Results

While the encoding/decoding of individual images from the MNIST dataset left something to be desired, I was able to confirm that — yes — decoding a linear interpolation of two encoded images did produce a smooth and interesting (aka, not a “crossfade”) transition between them. 
