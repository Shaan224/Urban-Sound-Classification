# Urban-Sound-Classification

The Urban Sound Challenge is a classification contest provided by Vidhya Analytics with the purpose of introducing curious people to a real-world classification problem. After registered, one is provided with a dataset containing sounds from ten classes. For the training data set, the classes (targets) are given, but there is also a test dataset where the targets are unknown. Our task is to classify the test dataset correctly, and by uploading our results to Analytics Vidhya's webpage, they will return the accuracy score.

The data sets consist of a total number of 8732 sound samplings (5434 training
samplings and 3298 test samplings) with a constant sampling rate of 22050Hz. The
length of each sampling is maximum 4 seconds, and they are distributed between
ten classes, namely

* air conditioner

* car horn

* children playing

* dog bark

* drilling

* engine idling

* gun shot

* jackhammer

* siren

* street music.

# Sound Analysis

In sound analysis, one can either analyze the raw sound files directly or convert
them to simpler files and hope we have not lost any essential features. A big
advantage with the latter, is that the dimensionality and complexity of the data
set is significantly reduced, but we will also loose some information. In this section
we will first introduce the Urban Sound Challenge, and then look at the sounds
from a time perspective, frequency perspective and see how to extract information.
Finally, we take a look at some of the samplings and see if we can classify them
with the naked eye.


# Time Domain

Sounds are longitudinal waves which actually are different pressures in the air.
They are usually represented as a function in the time domain, which means
how the pressure vary per time. This function is obviously continuous, but since
computers represent functions as arrays, we cannot save all the information.

![](https://github.com/Shaan224/Urban-Sound-Classification/blob/master/Screenshot%20(239).png)

When sampling sound, we need to choose a high enough sampling. Here,
the red line is the true sound and the blue dots are sampling points. The sampling
rate here is 40 Hz, which is quite low and just for illustration.

# Frequency Domain

Sometimes, a frequency domain gives a better picture than the time domain. On
one hand, one looses the time dependency, but on the other one gets information
about which frequencies that are found in the wave. To go from the time domain
to the frequency domain, one needs to use Fourier transformations - read more on <https://en.wikipedia.org/wiki/Fourier_transform>

![](https://github.com/Shaan224/Urban-Sound-Classification/blob/master/Screenshot%20(240).png)

Time picture of three harmonic functions; sin(2πt), sin(4πt) and sin(8πt)
(upper part) and the corresponding frequency pictures (lower part).

# Extract Features

There are many ways we can extract features from sound, but the most interesting
are those that are based on how humans extract information. First the spectrogram
method will be described briefly, and we thereafter turn to the mel frequency
ceptral coefficients method.

## Spectrograms

As we have seen above, we are missing the frequency information in the time
domain, and time information in the frequency domain. Is it possible to get the
best of both worlds? This is what we try when creating a spectrogram, which is a
time-frequency representation.

The procedure goes like this: we divide the spectrum into N frames and Fourier
transform each frame. Thereafter we collect all the frequency domains in a NxMmatrix, where M is the length of each frequency domain. We are then left with an
image which hopefully hold the important information, and since Hidden Markov

Models are known to implicity model spectrograms for speech recognition, we have
a reason to believe it will work.

## Mel Frequency Cepstral Coefficients (MFCCs)

MFCCs are features widely used in automatic speech and speaker recognition.
[Lyo13] They are inspired by how the human ear extracts features, and has been
the main feature extraction tool for sound recognition since it was introduced in
the 1980’s.

It actually takes advantage of the spectrogram, and use it to reveal the most
dominant frequencies. Thereafter, we sum the energy in each frame and take the
logarithm, which is the way an ear works. Those energies are then what we call
the Mel Frequency Ceptral Coefficients. Unlike the spectrogram, we are now left
with a single array of information from the spectrum.

![](https://github.com/Shaan224/Urban-Sound-Classification/blob/master/Screenshot%20(241).png)

Time domains (top), spectrograms (middle) and MFCCs (bottom) for
two random sound files, more specifically dog bark on the left and jackhammer on
the right.


However, sometimes two time domains in the same category look totally different, for example if one compares the jackhammer above with the one below. Will it be possible for our program to understand that they are in the
same category? Also sometimes two time domain from different categories look
similar, as we can see below. Can we really distinguish between them?

![](https://github.com/Shaan224/Urban-Sound-Classification/blob/master/Screenshot%20(242).png)

Time domains (top), spectrograms (middle) and MFCCs (bottom) for
two random sound files, more specifically jackhammer on the left and drilling on
the right.


# Methodology

To extract features from our dataset (dimensionality reduction), I used a spectrogram in the CNN case, and Mel Frequency Ceptral Coefficients (MFCCs) for FNN and RNN. For RNN I mainly tried the Long Short-Term Memory (LSTM) network and the Gated Recurrent Unit (GRU) network.

All the neural networks were more or less able to recognize the training set, but it was a significantly difference in validation accuracy. FNN provided the highest validation accuracy of 92.37% and Training accuracy of 91%.

