---
layout: post
title:  “Inclination angles and a uniform distribution for isotropy”
date:   2018-02-18 23:00
categories: [math, analysis]
permalink: /archivers/uniforminclination
---

Here’s a little math problem that I’ve done enough times to warrant writing down somewhere I won’t lose it.

A useful quantity to know in many astronomical situations is the inclination angle of some system.  In my work, this is usually the orientation angle of a stellar rotation axis or the orbital plane of a binary star system. This is the orientation angle with respect to the line of sight of the observer and shown in this diagram:

<img src="http://keatonb.github.io/img/inclination.png” />

To help remember how the inclination angle, \\(i\\), is defined, I use the mnemonic that an elliptical orbit with zero inclination <i>looks</i> like a zero.  The angle is defined for \\(0\deg < i < 90\deg\\) (\\(0 < i < \pi\\) radians), since we are only interested in the projected magnitude along our line of sight.

Our measurements are often only sensitive to the projections of quantities onto our line of sight or onto the “plane” of the sky.  Since the projection does not equal the full magnitude of the quantity under investigation, this leads to so-called \\(\sin{i}\\) ambiguities.  Such ambiguous measurements can be interpreted as lower/upper limits on physical value, interpreted statistically in the context of many objects, or used in conjunction with other measurements to empirically constrain \\(i\\).

Without good reason to think otherwise, we should expect a random object to be oriented randomly.  Our knowledge of its inclination angle can be represented by an ``isotropic’’ probability density function (p.d.f), meaning all orientations are equally likely.  The figure below will help us to write this down so that it is mathematically useful.

<img src="http://keatonb.github.io/img/inclinationsolidangle.png” />

I’ve only drawn the observer-side hemisphere because the other half is merely a reflection.  If a vector is equally likely to be oriented toward any part of the sphere, the relative likelihood that it falls within some small range \\(di\\) of a specific inclination angle is proportional to the area on the unit sphere covered by that range of angles.  According to the figure above, the [solid angle](https://en.wikipedia.org/wiki/Solid_angle) subtended by the inclination range \\(i\\) to \\(di\\) equals \\(2\pi\sin{i}di\\).  We can write down the p.d.f. of isotropic inclination angles as

\\[f(i) =
\begin{cases}
0,  & i < \pi \\
\sin{i},  & 0 \le i \le \pi \\
0, & i > \pi,
\end{cases}\\]

which we have normalized so that \\(\int_{-\infty}^{\infty}f(i)di = 1\\).

I’m pausing here to see if this has all rendered before I write anymore…
