---
layout: post
title:  "What's the expected average value of a noisy amplitude spectrum?"
date:   2021-05-18 16:00
categories: [analysis]
permalink: /archivers/meanamplitude
---

I find myself working out the relationship between the noise in time series data to the noise in the periodogram represented as an amplitude spectrum (Fourier transform) fairly often, so I'm writing it down somewhere I won't lose it. I agree with the statement from ["Asteroseismic Data Analysis: Foundations and Techniques"](https://princeton.universitypressscholarship.com/view/10.23943/princeton/9780691162928.001.0001/upso-9780691162928) by Sarbani Basu and Bill Chaplin (Section 5.1.4)

> Belt-and-braces checks of the [power spectrum] calibrations may be made by considering limiting dataset cases comprising of either white noise or commensurate sinusoids. It is extremely useful to be able to make a quick back-of-the-envelope prediction of the expected power spectral density for the white-noise case. This provides an estimate of the shot noise background we would expect given known photon-counting rates.

They then show that for the power spectrum, calibrated via Parseval's theorem (total power in time series = total power in power spectrum), the average power per frequency bin is \\(2\sigma^2/N\\), where \\(\sigma\\) is the relative flux uncertainty (assumed stationary), and \\(N\\) is the number of time series points.  A nice result for the power spectrum, where white noise will be distributed about this level as \\(\chi^2\\) with two degrees of freedom. This distribution has some convenient properties that make it relatively easy to work with, and I demonstrated how to work with this distribution in my post ["Fitting features in a power spectrum with least squares and MLE"](http://keatonb.github.io/archivers/powerspectrumfits).

However, in many studies of time series variability, the amplitude spectrum is preferred to the power spectrum, and is simply the square-root of the power spectrum (depending on normalization convention, but at least proportional). I prefer the amplitude spectrum when I am fitting models in the time domain rather than the frequency domain. The height of a peak in the amplitude spectrum can tell you directly the amplitude and frequency of a best-fit sinusoid in that frequency bin. The  highest points in the power spectrum (squared amplitude spectrum) will also appear to stick out higher above the noise by eye, so my colleague Michel Breger taught me another, perhaps psychological reason to prefer working with the amplitude spectrum: you'll be less likely to convince yourself that a noise peak is real when looking at the amplitude spectrum. I haven't generally taken the other side of his advice (given at least part jokingly): plot the power spectrum in publications to convince readers that the signals you claim to have detected are real!

Despite how much I want it to be true, the average value of the white noise amplitude spectrum is not the square-root of the average noise in the power spectrum. The square root is not a linear operator, and therefore the shape of the noise distribution changes... for the worse.  While the noise in the power spectrum was distributed as \\(\chi^2\\) with two degrees of freedom, the noise in the amplitude spectrum is distributed as a [Chi distribution](https://en.wikipedia.org/wiki/Chi_distribution) with two degrees of freedom, where the mean value involves some ugly gamma functions. Long story short, we have to multiply the square-root of the average noise in the power spectrum, \\(\sigma\sqrt{2/N}\\), by the ratio of the mean to the scale parameter for a \\(\chi\\), 2 d.o.f. distribution. We can get this coefficient easily with [SciPy](https://www.scipy.org/). 

```python
from scipy.stats import chi
print(chi.stats(df=2, loc=0, scale=1, moments='m'))
```

> 1.2533141373155003


