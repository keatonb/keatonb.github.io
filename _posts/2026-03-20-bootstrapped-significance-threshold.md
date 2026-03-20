---
layout: post
title:  "Bootstrapping a significance threshold for periodogram analysis"
date:   2026-03-20 11:00
categories: [analysis]
permalink: /archivers/sigthresh
---

In periodogram analysis of data with measurement errors, one must make a critical decision about where to draw the line between what is interpreted as a real signal and what could be just a signature of noise. Typically I am analyzing time series measurements of brightnesses of stars with the Fourier transform or the Lomb-Scargle periodogram, and I want to know if the star is varying significantly in brightness or if the measurements are consistent with what we expect for noisy measurements of a constant-brightness source. My approach is to consider whether any peaks in the periodogram are tall enough to be exceedingly unlikely to represent noise. A periodogram peak must be as tall as the "significance threshold" for me to believe it represents a real signal.

<img src="https://i.imgur.com/hriXTOG.jpeg" width="30%" alt="Child is not tall enough to ride the amusement park ride, representing insignificant noise." />

I go over some of the basics of periodogram analysis in my previous post ["How data sampling affects the Fourier transform periodogram."](https://keatonb.github.io/archivers/ft) The animated periodograms in that post include a line at 4 times the average amplitude in the periodogram as an approximate significance threshold, but careful data analysis should adopt a threshold level that is deliberately chosen to be appropriate for the problem at hand. This post will demonstrate how to calculate a statistical threshold with bootstrap resampling.

Consider the periodogram below of a pulsating white dwarf star observed by NASA's TESS satellite under the target name TIC 188087204.

<img src="http://keatonb.github.io/img/TIC188087204.png" width="70%" />

This is the amplitude spectrum (square root of the power spectrum on the y axis, in units of parts-per-thousand), showing what the best-fit amplitude is for a series of sinusoids with different frequencies (sampled on the x axis in units of microHertz from zero to the [Nyquist frequency](https://keatonb.github.io/archivers/pyquist)). This is the frequency-domain representation of 60 days of brightness measurements of a white dwarf star taken every 2 minutes, with considerable measurement noise affecting each data point. The noise manifests in the periodogram as a noise floor of peaks with a range of heights distributed across all sampled frequencies. This might look like the grass of an unkempt lawn growing along the bottom of the plot. It is apparent in the periodogram shown above that some peaks stick out taller than the rest. The question of significance, in the lawn analogy, is whether the tallest peaks are dandelions (signal) or just the tallest of the many blades of grass (noise).

If we make some simple (but not exactly correct) assumptions about the time series data---that each measurement is evenly spaced in time and has uncorrelated Gaussian-random error---then the amplitude values in the periodogram are distributed as a Chi distribution with two degrees of freedom (I describe this a bit more [here](https://keatonb.github.io/archivers/powerspectrumfits) and [here](https://keatonb.github.io/archivers/meanamplitude)).

 