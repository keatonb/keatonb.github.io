---
layout: post
title:  "Bootstrapping a Significance Threshold for Periodogram Analysis"
date:   2026-03-20 16:00
categories: [analysis]
permalink: /archivers/sigthresh
---

In periodogram analysis of data with measurement errors, one must make a critical decision about where to draw the line between what is interpreted as a real signal and what could be just a signature of noise. Typically I am analyzing time series measurements of brightnesses of stars with the Fourier transform or the Lomb-Scargle periodogram, and I want to know if the star is varying significantly in brightness or if the measurements are consistent with what we expect for noisy measurements of a constant-brightness source. My approach is to consider whether any peaks in the periodogram are tall enough to be exceedingly unlikely to represent noise. A periodogram peak must be as tall as the "significance threshold" for me to believe it represents a real signal.

<img src="https://i.imgur.com/hriXTOG.jpeg" width="35%" alt="Child is not tall enough to ride the amusement park ride." />

I go over some of the basics of periodogram analysis in my previous post ["How data sampling affects the Fourier transform periodogram"](https://keatonb.github.io/archivers/ft). The animated periodograms in that post include a line at 4 times the average amplitude in the periodogram as an approximate significance threshold, but careful data analysis should adopt a threshold level that is deliberately chosen to be appropriate for the problem at hand. This post will demonstrate how to calculate an appropriate threshold with bootstrap resampling.

Consider the periodogram below of a pulsating white dwarf star observed by NASA's TESS satellite under the target name TIC 188087204.

<img src="http://keatonb.github.io/img/TIC188087204.png" width="70%" />

This is the amplitude spectrum (square root of the power spectrum on the y axis, in units of parts-per-thousand), showing what the best-fit amplitude is for a series of sinusoids with different frequencies (sampled on the x axis in units of microHertz from zero to the [Nyquist](https://keatonb.github.io/archivers/pyquist) frequency).
