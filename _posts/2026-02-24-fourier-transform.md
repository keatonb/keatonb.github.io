---
layout: post
title:  "How data sampling affects the Fourier Transform"
date:   2026-02-24 22:00
categories: [analysis]
permalink: /archivers/ft
---

The Fourier transform and related (e.g., Lomb-Scargle) periodograms are incredibly valuable tools for transforming and interpreting data. I am typically concerned with using the periodogram to detect and characterize variations in time series data, such as recorded brightnesses of stars. I will demonstrate four key considerations for understanding how the qualities of your data affect the representation of signals in the periodogram.

For a deeper dive on the statistical considerations for the Fourier-based Lomb-Scargle periodogram, see 
(["Understanding the Lomb-Scargle Periodogram"](https://ui.adsabs.harvard.edu/abs/2018ApJS..236...16V/abstract)) from Jake VanderPlas. ([Pyriod](https://github.com/keatonb/Pyriod)) is a Python package I wrote for interactive Fourier analysis of time series data in a Jupyter notebook. The following are essential considerations for performing such an analysis reliably. 

## Some preliminaries

The Fourier transform periodogram can be though of as essentially representing the best-fit amplitudes of sinusoids fitted to some data with many different test frequencies. Sometimes the periodogram is displayed in terms of power (or power density), but I prefer best-fit amplitude versus frequency.

Here I am thinking primarily about time series data collected with regular periodic time sampling, with a new observation made every \\(\Delta t\\) seconds or days. This will produce the most readily-interpretable data set with fairly straightforward Fourier behavior, such as a well-defined Nyquist frequency. Understanding how to interpret a periodogram of evenly sampled data is an essential preliminary for understanding any unevenly sampled data. 

An important aspect of Fourier analysis is separating signals of interest from the noise. Time series measurements will each have some random measurement errors. If this noise is uncorrelated and similar throughout the data set, the contribution of noise to the periodogram will be a noise floor of peaks distributed across all sampled frequencies. The peaks will follow some disribution (\\(\chi^2\\) with two degrees of freedom if the noise is Gaussian) that falls off exponentially to high amplitude, so it's unlikely for the noise in your data set to produce a peak that is "very tall." A very tall peak is more likely to represent a real signal, and we might interpret peaks that rise above some significance threshold in the periodogram to be real signals we wish to analyze. Where to set such a threshold depends on your data and the problem at hand. Often it is defined as some factor above the average peak height (noise level; \\(\langle A\rangle\\)) in the periodogram. For demonstration purposes, these examples show a threshold four times above the average amplitude in the periodogram, 4\\(\langle A\rangle\\).

## Signal to noise

The periodogram is powerful for its ability to reveal periodic signals in noisy data that can't be seen by eye. This is especially true when you have a lot of data spanning many cycles of variations. To show how the sensitivity to low-amplitude signals improves as more data are collected, this animation shows the evolution of a simulated noise-dominated time series (top) and its Fourier transform (bottom) as more data are collected.

<img src="http://keatonb.github.io/img/SNR.gif" width="50%" />

Initially, all periodogram peaks have similar heights representing that the data are consistent with white noise, with none rising above the 4\\(\langle A\rangle\\) significance threshold. As more data are collected, the noise peaks decrease in amplitude and the significance threshold lowers until one peak is revealed to rise significantly above the rest. As more data are collected (\\(N\\) data points), the average noise level decreases as \\(1/\sqrt{N}\\), and the signal-to-noise ratio increases as \\(\sqrt{N}\\).

## Frequency resolution

You'll also notice in the animation above that the peaks get narrower as more data is collected. The frequency resolution goes as \\(1/T\\), where \\(T\\) is the total length of the time series. With continuous, evenly sampled data, the a coherent sinusoidal signal will have the appearance of a  ([sinc function](https://en.wikipedia.org/wiki/Sinc_function)) in the amplitude spectrum, with the peak dropping to zero at every multiple of \\(1/T\\) from the center of the peak. Under these conditions, periodogram values separated by multiples of \\(1/T\\) are not correlated and are said to be "independent" of each other.

If two signals are closer to each other than the frequency resolution, they will appear as one peak until enough data is collected to resolve them. This animation shows how the periodogram of a simulated data set with two closely spaced frequencies evolves as more data is collected.

<img src="http://keatonb.github.io/img/Res.gif" width="50%" />

When the time baseline \\(T\\) is too short to resolve these two signals, they appear as just one wide peak in the periodogram. As more data are collected, eventually the resolution gets smaller than the frequency difference, and the unresolved peak splits into two to reveal the two signal frequencies.

Having two signals with similar frequencies is the situation where we get the ([beating](https://en.wikipedia.org/wiki/Beat_(acoustics))) phemomenon. Superimposing these two sine waves with slightly different frequencies, the phase difference between the signals will slowly evolve, alternating between times of constructive and destructive interference. This is visible as the envelope of variability amplitude seen in the time series plot. The frequency of amplitude modulation from beating is the beat frequency \\(f_{beat} =  \lvert f_1 - f_2\rvert \\). The condition for resolving the peaks is that you observe at least one full beat cycle with an observing duration exceeding \\(1/f_{beat}\\). Before the peaks are strictly resolved, the shape of the peak will appear distorted from a sinc function, revealing that you've got more than just a single coherent sinusoid but you won't be able to pin down exactly what frequencies are involved.

## Nyquist aliasing

(coming soon)

<img src="http://keatonb.github.io/img/Nyq.gif" width="50%" />

## Aliasing from gaps in the data

(coming soon)

<img src="http://keatonb.github.io/img/Alias.gif" width="50%" />
