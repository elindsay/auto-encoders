# Experiments with Variational Autoencoders
I had been generating small video clips with Runway ML, which generates 4-16 second video clips. I wanted to be able to connect different clips together using a smooth transition, as opposed to an abrupt cut. Most of the videos were fairly psychedelic in nature, so it wasn’t really important to create something “normal” looking, but more of like, a brief surrealist transition 

The issue, is that while many of the generative AI tools I was using on the internet could “imagine” images from scratch, they couldn’t interpolate well between two images. I’d recently learned about variational autoencoders, and was fascinated by the idea of “encoding” an image into a “latent space”. I thought that, using a (probably linear) interpolation between two encoded images in the latent space, it would be possible to create a continuous (if possibly slightly surreal) transition between two images. 


## 1 - MNIST Variational Autoencoder
The goal of this project, was to see if I was correct — that a linear interpolation between the encoded values of two images would produce a smooth transition when those values were decoded. (Note: I actually attempted this first using a simple autoencoder, not a variational autoencoder, and realized that the transition simply looked like a crossfade, which was not interesting to me given that you could easily generate such an effect in a video editing program.)

Code for this project [is here](https://colab.research.google.com/drive/1-03eB9covktokFlcFBpvE-LnXg-invpw "Google's Colab")

Most of this code is very similar to the variational autoencoder code from the MNIST lab of the [Generative Deep Learning with Tensorflow](https://www.coursera.org/learn/generative-deep-learning-with-tensorflow) lab in Coursera the main difference being that it uses convolutional layers for the autoencoder. I kept the latent space two dimensional, like it had been in the original lab, because it seemed to give adequate results for my purposes, and it would be easier for me to debug than a higher dimensional space. 

### Results

While the encoding/decoding of individual images from the MNIST dataset left something to be desired, I was able to confirm that — yes — decoding a linear interpolation of two encoded images did produce a smooth and interesting (aka, not a “crossfade”) transition between them. 

Here are two original images of a 5 and a 2 from the MNIST dataset:

![Five And Two](https://storage.googleapis.com/psychedelic-vae/github-pages-assets/five-and-two-original.png)

They were each encoded by the encoder portion of the VAE as a two dimensional tensor, with the values `[-0.15716521  0.01044209]` and `[-0.580478   0.7451283]`.

I then linearly interpolated 8 two dimensional values, between the two encoded values, while including the original values at the start and the beginning, to create a 10 by 2 tensor of values that could be decoded. The first and last images would be the decoded versions of the original images, and the middle 8 images would be the interpolation between the two. 

Here are the interpolated values:

```
[
 [-0.15716521  0.01044209],
 [-0.20419997  0.0920739 ],
 [-0.2512347  0.1737057],
 [-0.29826948  0.25533748],
 [-0.34530425  0.3369693 ],
 [-0.392339   0.4186011],
 [-0.43937373  0.5002329 ],
 [-0.48640847  0.58186466],
 [-0.5334433  0.6634965],
 [-0.580478   0.7451283]
]
```

And, this is what those values looked like when run through the decoder:

![Five And Two Interpolated](https://storage.googleapis.com/psychedelic-vae/github-pages-assets/five-and-two-interpolation.png)

Now, while the encoded and decoded values of the original images was imperfect (the 5 seemed to be interpreted as more of a 3 than a 5 by the encoding) this did confirm my initial theory, that it was possible to create a more interesting interpolation between the two images than a simple crossfade. For instance, you can see features of the 2 developing later on during the transition which would have been visually present much earlier if it were just cross fading. 

Here are a few more examples of random images from the MNIST dataset being interpolated with the VAE:

![Zero And Seven](https://storage.googleapis.com/psychedelic-vae/github-pages-assets/zero-and-seven-original.png)

![Zero And Seven Interpoalted](https://storage.googleapis.com/psychedelic-vae/github-pages-assets/zero-and-seven-interpolation.png)

I thought this one was particularly interesting, because you can see the decoded values go through more numbers during the transition (looks like 0 → 3 → 8 → 7/9). This is because zero and seven were encoded in very different areas of the latent space (zero was encoded as [0.46782172, 1.1733856]  and seven was encoded as [-0.32225293, -1.1759665 ]) and as the linear interpolation produced values that were more associated with other numbers, those numbers showed up in the decoded output. Specifically, the encoded values that were closer to 0.0 tend to be decoded as images that look like 3s and 8s, which you can see in the output.

![Nine And Five](https://storage.googleapis.com/psychedelic-vae/github-pages-assets/nine-and-five-original.png)

![Nine And Five Interpoalted](https://storage.googleapis.com/psychedelic-vae/github-pages-assets/nine-and-five-interpolation.png)

![Zero And Six](https://storage.googleapis.com/psychedelic-vae/github-pages-assets/zero-and-six-original.png)

![Zero And Six Interpoalted](https://storage.googleapis.com/psychedelic-vae/github-pages-assets/zero-and-six-interpolation.png)

Overall, this was sufficient for what I was looking for. I didn’t particularly want to invest time into increasing the efficacy of the encoding and decoding of the MNIST dataset because I wanted to move on to color images — and, also, for my purposes a slightly imperfect encoding/decoding I felt was likely to be sufficient. I didn’t be generating static images that would be inspected for a long time, but rather movie frames that would only be shown for a fraction of a second, so perfection of any one image was less important than the smooth transition between images.
