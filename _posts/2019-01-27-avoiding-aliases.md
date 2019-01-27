---
layout: post
title:  "Strategically timing observations to avoid frequency aliases"
date:   2019-01-27 20:00
categories: [observing]
permalink: /archivers/avoidingaliases
---

In time domain research, it is well understood that the pattern of
observations in time manifests itself in the frequency domain as a
set of aliases that can confuse your identification of the intrinsic
frequencies of interest. Conversely, if you want to avoid confusion in
your frequency measurements, you should time your observations
strategically.

Telescope time is expensive and competitive, so the observer has a
responsibility to use that time efficiently. Sometimes more
observations is not better than smart observations. I want to review
an important paper that helped me learn how to observe smarter and
explain how to apply this understanding to a problem from my own
work. The paper, ["Variable stars: Which Nyquist frequency?" by
L. Eyer and P. Bartholdi (1999, A&AS, 135, 1)]
(http://adsabs.harvard.edu/abs/1999A%26AS..135....1E), is one of my
favorites at only 3 pages long, and I think every time domain
astronomer should be sure to understand it. It makes two main points:

1. The Nyquist frequency (see my previous post
[Nyquist analysis and the pyquist module](http://keatonb.github.io/archivers/pyquist))
for irregularly sampled data is \\(1/(2p)\\), where \\(p\\) is the
greatest common divisor of all of spacings in time between all pairs
of observations. This is often very much higher than incorrect values
commonly quoted in the literature that replace \\(p\\) with either the
average or smallest time between observations.
2. The heights of aliases in the observational spectral window (the
   characteristic signature about a coherent signal in the Fourier
   domain) are equal to the offset of the center of gravity of the
   observation times phase-folded on the alias frequency.

In this post, I focus on this second point and explore how this
can be used to obtain better data. First, I will demonstrate the
argument that I put forward in a
[white paper](https://arxiv.org/abs/1812.03142) for this
effect to be considered while executing the sparse
time-domain observations of the
[Large Synoptic Survey Telescope](https://www.lsst.org/) (LSST).
Then I will provide another simple example from my work of how the
strategy for observing an individual target can be improved by
considering the connection between observation timing and frequency
aliases.

LSST will
observe every part of the southern sky several hundred times over a
ten year survey. They put out a
[call for white papers](https://www.lsst.org/call-whitepaper-2018)
to weigh in on how these observations should be collected (the survey
cadence), and they have also developed a sophisticated
[simulation framework](https://www.lsst.org/scientists/simulations)
for studying the effect of this cadence on the science that can be
done with the data.

The figure below shows the spectral window (the Fourier transform of
a constant value sampled when the observations are made) of a field in
a simulated realization of the main LSST survey after three
years. Peaks in the spectral
window are called aliases and they can cause intrinsic signal frequencies to
be mis-identified as being offset by the alias frequency, especially
in noisy data. The highest peak in this example at 11.6 micro-Hertz is
the practically unavoidable diurnal alias of single-site observing
that is limited to night-time operations.  Basically, because you
can't observe during the day, there's some ambiguity about exactly how
many cycles of some periodic variation you missed when you weren't
observing.  However, the other aliases present in this spectral window
are easier to avoid and result from some other strict regularity of
observations being enforced.

<img src="http://keatonb.github.io/img/3yearsLSST.png" />

The polar plot inset in this figure demonstrated
[Eyer and Bartholdi](http://adsabs.harvard.edu/abs/1999A%26AS..135....1E)'s 
point about alias peak heights equaling the center of gravity of the
phase-folded time sampling. Here I've wrapped the observation times
around a circle, folded on one alias period of 2.18 hours. The
relative concentration of these points near the bottom of the circle
cause the center of gravity of these points to be offset by nearly
half the radius of the circle, which is equal to the alias peak. The
same goes for every frequency of the spectral window.

The actionable takeaway is that future observations can be timed to
move this center of gravity toward the middle of the circle, reducing
the alias amplitudes. In the next figure, I show how the timing of a
new observation will change each of the marked alias amplitudes
from the previous figure. Green mean the amplitude is reduced, and red
means that alias grows in amplitude.  The bottom panel shows the
effect on the combined heights of the set of marked aliases. 
 
<img src="http://keatonb.github.io/img/aliaschanges.png" />

We see that the situation is improved as long as the next observation
does not reinforce the strong concentration of observations near
hour 6. Aliasing can be made less of a problem for time domain
astronomy if this effect is taken into consideration as the timing of
survey observations is being decided.




