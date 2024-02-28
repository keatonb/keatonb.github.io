---
layout: post
title:  "Three Statistical Tests for Average Spacing Among Numbers"
date:   2024-02-28 10:00
categories: [analysis,python]
permalink: /archivers/mps
---

The general problem I'm interested in today is whether a set of values is distributed such that there is some regularity in their spacing, and how to identify that average spacing. This may be an incomplete set of measurements belonging to an evenly spaced pattern, in the way that 16, 17, 19, 23, 24, 28 belong to a set of numbers evenly spaced by 1. The values may not be strictly evenly spaced, and may deviate from an even *average* spacing. And the set of numbers could contain a mix of values, only some of which follow an even spacing, or which follow multiple trends with even average spacings.

This situation comes up a lot in my research on white dwarf pulsations. The periods of gravity-mode pulsations of white dwarfs are mostly expected to follow a pattern of even period spacing as part of an overtone series, though individual periods will deviate from that average pattern. And only a subset of mode periods are typically detected. Structure in these measurements is rarely so obvious that it jumps right out at you, and so it is common in the field to employ one or more of three statistical tests for mean period spacings: the Inverse Variance test ([O'Donoghue 1994, MNRAS, 270, 222](https://ui.adsabs.harvard.edu/abs/1994MNRAS.270..222O)), the Kolmogorov-Smirnov test ([Kawaler 1988, Advances in Helio- and Asteroseismology, 123, 329](https://ui.adsabs.harvard.edu/abs/1988IAUS..123..329K)), and the Fourier Transform test ([Winget et al. 1991, ApJ, 378, 326](https://ui.adsabs.harvard.edu/abs/1991ApJ...378..326W/)). 

I will introduce and explain how each of these tests works, as I think they are often misunderstood, even in our research subfield where they are widely used. It doesn't help that they are not usually described in much detail where they are used in the literature. I will dispel the the misconception I've heard a few times that they are "mathematically identical" (their results are not identical, so they can't be exactly the same). However, they are all doing something very similar, if not obvious at first glance. Conventional wisdom is to use all three to ensure that they all report the same average spacing.

These tests were all used in the [analysis of pulsations measured from the TESS spacecraft of the white dwarf WD 0158-160](https://ui.adsabs.harvard.edu/abs/2019A%26A...632A..42B) (Figure 3), where they all supported a mean period spacing of 38 seconds between measured pulsation periods of 485.527, 557.649, 597.554, 604.642, 640.533, 678.433, 749.27, and 865.97 seconds. I'll use those values for this demonstration. I keep the code at a minimum, but eventually I will link to some code on GitHub.

# Searching for Periodicity

All three methods are applied to a range of possible spacings, evaluating how much each spacing appear to be present compared to other spacings that were tested. And all three tests start somewhere similar, essentially wrapping the values of interest on a number line at each test period. This puts the values in "phase" from zero to one, folded on the test period, to see if values tend to clump together, suggesting that values line up on the test period, or they spread out all over in phase when the test period does not describe underlying structure in the data. For a set of values you want to find periodicity in, like the pulsation periods above, you can wrap the values on a test period, \\(p\\), using the [modulo operator](https://en.wikipedia.org/wiki/Modulo), which in Python is the \\(%\\) symbol. The modulo operator returns the remainder after division, so 28 % 5 = 3, because 5 goes into 28 five times with a remainder of 3. Dividing by the test period forces the range of values to go between 0-1.

```python
phase = (values % testperiod)/testperiod # values wrapped on test period, between 0-1
```
Graphically, the phase can be represented on a polar plot with angles ranging from \\(0\\) to  \\(2\pi\\). Here we show how the example values above wrap around the numberline on two different test periods.

<img src="http://keatonb.github.io/img/mps_phasewrapped.png" width="50%" />

On the left, values are somewhat randomly distributed in phase for a test period of 26.5 seconds, indicating that the the values do not align on this periodicity. On the right, however, values clump together in one part of the phase diagram, suggesting that they do align somewhat with a spacing of 38.1 seconds. While all three methods essentially convert values to phases wrapped on test periods in search of values concentrating more in phase on test periods that are present in the data, the three methods differ in how they quantify how bunched up the values are in phase.


# Inverse Variance Test

The easiest to understand is the inverse variance test. With values wrapped on a test period, we want to know how concentrated vs spread out these values are in phase. That's the kind of thing that the [variance](https://en.wikipedia.org/wiki/Variance) measures! Variance is the average squared difference from the mean. The inverse of the variance is large when the variance is small, and small when the variance is large.

There's just one gotcha before we ask numpy to compute the variance... there's a pesky boundary on our numberline where values wrap discontinously from 1 back to zero. Calculating the variance across this boundary produces results that are difficult to interpret. To avoid this, we can shift all the numbers in phase so they're centered on average around 0.5, and we compute the variance relative to this mean value that is as far from the 1-0 boundary as we can get. Even that's not trivial, because taking the mean of numbers that cross the 1-0 can yield confusing results. I estimate phase angle by computing the angle of the *vector* average location of values on the polar plot. Centering on the angle of this average vector (marked with a black triangle), we get the plots below. These are displayed like a clock, with the 1-0 boundary at 12:00, so we force the average to be at 6:00. The orange color shades one standard deviation (square root of the variance), to either side of the average. We see how this metric measures the concentration of values in phase on the test period.

<img src="http://keatonb.github.io/img/mps_IVphase.png" width="50%" />

Testing a range of periods that could be present in the data produces a periodogram comparing concentration as a function of test period.

<img src="http://keatonb.github.io/img/mps_ivspec.png" width="75%" />

This reaches a peak value near 38 seconds, where we saw values clumping together in the right-side panels of the figures above. 

At the risk of just completely overdoing it, here's an animation with a graphical representation of inverse variance as a function of test period, showing how this IV periodogram is calculated for the full range of periods considered.

<img src="http://keatonb.github.io/img/IV.gif" width="95%" />

# Kolmogorov-Smirnov Test

The [K-S test](https://en.wikipedia.org/wiki/Kolmogorov%E2%80%93Smirnov_test) is something anyone doing statistics should be familiar with. It tests how consistent a set of values is with being drawn randomly from a given distribution, or it can be used to test whether two sets of numbers are consistent with being drawn from the same underlying distribution. We do the former here, comparing our phase-wrapped values to a uniform distribution between 0 and 1. If our test period does not cause values to concentrate in phase, they should be more or less randomly distributed as if they were drawn from a uniform distribution. We call the idea that these values are consistent with being random values the "null hypothesis." If the distribution in phase is not consistent with a uniform distribution, we can reject the null hypothesis and claim that there is structure at the test period.

The K-S test makes this assessment by comparing [cumulative probability distributions](https://en.wikipedia.org/wiki/Cumulative_distribution_function). This is like an integrated histogram, but without any binning. 


<img src="http://keatonb.github.io/img/mps_ks_cdf.png" width="75%" />


