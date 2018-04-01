---
layout: post
title:  "Generating Citable Paper Names with Recurrent Neural Networks"
date:   2018-04-01 15:00
categories: machine learning
permalink: /archivers/papernames
---

Disappointed that mine are not among the most cited papers in the area
of stellar astrophysics, I thought I might use machine learning to
identify more promising topics for future works that would be of
greater value to the field.  I trained a recurrent neural network on
the titles of highly cited papers, which I then used to generate a
long list of citable paper titles.

I used the 
[`ads` python module](https://ads.readthedocs.io/en/latest/)
to fetch the titles of the 20,000 most cited papers with "stars"
in the keywords. Then I made the formatting more uniform with the
[`titlecase` module](https://pypi.python.org/pypi/titlecase).

I used the
[TensorFlow-Char-RNN](https://github.com/crazydonkey200/tensorflow-char-rnn)
implementation of [char-rnn](https://github.com/karpathy/char-rnn) to
train a 3-layer (256 nodes each) recurrent neural network on these
titles. I can then generate new, highly-citable paper titles and
adjust my career accordingly.

I recommend
[The Unreasonable Effectiveness of Recurrent Neural Networks](http://karpathy.github.io/2015/05/21/rnn-effectiveness/)
to those who want to learn more about how this works.  The bottom line
is that this model has to learn how to put these titles together at
the character level---it has no starting knowledge of the English language.

I generated a very long list of high quality titles.  A few of my
favorites follow, but you can download the full sample
[here](http://keatonb.github.io/docs/adstitleoutput.txt).

> Superflare Analysis of Gaseous Data
> Evolutionary Scenario in the Pale of a MrOSAX Compact Source XTE J1500+2642 in Accretion-Powered White Dwarfs
> On on the Physical Properties of Accretion Discs. I. Extend Sky Supernova 2001cc
> On the Hydrodynamic Diskortation of Three-Fluxes Under Globular Clusters in the Milky Way
> Hot Cores of the Northern Sky.
> Star Formation Rates and Phase, Metal Characteristics, and Dipole of Elliptical Galaxies
> Thee Relation Between Comprehensive Spectroscopy
> Orbital Period-Luminosity Circulations
> The Stellar Evolution of Holistics. The Chemically Mitroch Observations of Intermediate Gravity.
> Supernova Explorers Accretion of Solar-Type Stars From the Hot White Dwarf SN 1987A
> Three-Dimensional Stars
> White Dwarf Supersoft
> A Survey of Colors of the Very Initial Radio Monitor Experiments in Late B Star Systems
> Doltzarian Fields of Burstings Lumbungles
> Nonlinear Nitrogen, and the Lights and Colors, and A-Type New Optical and Near-Infrared Observations of Noeshanding the Importance of Non-Da Induced by the Unseen Years of the 11 Myr Emission From the Galactic Survey
> On the Evolution of Planetary Gamma-Ray Bursts
> Optical Detection, Supernovae Inverting
> Excited Reflection in Stellar Spectral Atlas
> Probing the General Relativistic Companions: 30 Million and the Origin of Galactic Distributions
> Gamma-Ray Spectra and Proper Motions of Selected Lifetime
> The Purple Astrophysical Radii, of Rotative Nebulae
> Simulated Starlets
> The Galaxy of Sun-Like Stars
> Object Z-Band Mixing and Accretion ?
> Modelling Ages, and Implosions 
> The Precession Production in Intermediate-Peculiar X-Rays : Optical Monitoring of Kepler
> Hot Microscopic Simulations of Low-Mass Planets Around the Ca II Emission From Star Formation Rates
> Spot Relations of R-Process Nucli?
> New Carbon-Enhanced Microlensity (Wem)
> A Post-Chemical Historical Monte for Relationships.
> Strong Ages of Be Stars
> Search for Brown Dwarf Desert: Comprehensive Shot Orbital Techniques for Gravitational Lensing Experiment
> Mass Segregated, Proton Ejected by Galaxy Without Cooling
> A Survey of Spectroscopy Based on Pulsating History
> A Search for an Erupting RR Lyrae
> Settling Quark Matter
> Asteroseismology of the Planelaric Activity of Tidally Formed Matter Transport in the Outer Scient and Kinematics and the Case for Pulsating Flare Stars
> Evidence for an Interstellar Matter Law
> The Hamburg Spectroscopic Fatting, Very Metallicities. I. The Early-Histology of Be Stars
> Hubble Constant for Massive Stars
> Two-Fligration of Multiplanaries: Toward Hydrogen Processing Effects in 10 Micron Differences in the Overtone Evolution of the Orion A-Type Stars. VI. Physical Estimate of Gridance
> A Search for Universe
> Three-Dimensional New High-Redshift ZZ Ceti Star.
> Discovery of Stellar Winds From Bolometric Results. VI. Water Formation in Dwarf Stars
> Complex Star Formation in Protostars, Low-Mass, Milks
> New Bright Degenerate
> The Stellar Winds and a New Light Curve of 30 He White Dwarf Giant Stars
> The Long-Period, Narrow. III. Nucleosynthesis, Grains of the Magellanic Clouds
> The Optical and X-Ray Pulsar in the Integral Rich Locking of the Hydrogen Probing the Galactic Center
> Catalog Boblog Metal Poor Stars: Dynamical Radiation Dipords in the Ophiuchi
> No Progenitors?
> Further Wind
> Effects of Understanding White Dwarf Stars.



