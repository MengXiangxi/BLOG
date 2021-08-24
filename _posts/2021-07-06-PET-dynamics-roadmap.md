---
title: "Dynamic PET imaging itinerary"
categories:
  - Imaging
tags:
  - Dynamic Imaging
  - study note
date: 2021-07-06
layout: post
---

In this post, I am summarizing some materials on dynamic PET imaging, especially the compartment model analysis.

## Book chapters

- **Physics in Nuclear Medicine**, Simon R. Cherry et. al., Sanders, 2012 4th ed.

Chapter 21 of this book, "Tracer Kinetic Modeling", briefly discusses the transportation phenomenon and the compartment models.

- **Basic Sciences of Nuclear Medicine**, edited by Magdy M. Khalil, Springer, 2011.

Chapter 16 of this book, "Tracer Kinetic Modeling: Basics and Concepts", by Kjell Erlandsson from University College of London.

- **Emission Tomography: The Fundamentals of PET and SPECT**, edited by Miles N. Wernick and John N. Aarsvold, Elsevier, 2004.

Chapter 23 of this book, "Kinetic Modeling in Positron Emission Tomography", by E. D. Morris et. al..

- **Positron Emission Tomography: Basic Sciences**, edited by Dale L. Bailey et. al., Springer, 2005.

Chapter 6 of this book, "Tracer Kinetic Modeling in PET", by Richard E. Carson.

## Review Articles

- Hooker, Jacob M., and Richard E. Carson. "Human positron emission tomography neuroimaging." *Annual review of biomedical engineering* 21 (2019): 551-581.

In this review article, the authors summarized comprehensively all aspects of neuroimaging. Part of it discusses modeling in a straightforward way, however, it is based entirely on the reference tissue method.

## Software

- [KMtoolbox](https://github.com/mscipio/KMtoolbox)

KMToolbox is a MATLAB program to directly estimate various model parameters, directly measured from the 4D image. It is capable of processing both PET and DCE-MRI.

However, the major limitation is the lack of reliable documentations. Although the code is clearly written with ample comments, and the author [Michele Scipioni](https://mscipio.github.io/) provided some further information on the [Project Page](https://mscipio.github.io/project/kmtool/), there are still some confusing aspects, as the conventions and notations in kinetic modeling are not unified.

- [pmod](https://pmod.com/)

pmod is a commercial software built with JAVA, and is rather costly. I am not a big fan of pmod, but admittedly it provides some very useful application-specific shortcut solutions. The modules involved in kinetic modeling are [PKIN](https://www.pmod.com/web/?page_id=660) (parameter estimation, etc.) and [PXMOD](https://www.pmod.com/web/?page_id=667) (pixel/voxel-wise modeling). Admittedly, pmod integrates some kinetic models which save a lot of efforts.

## Representative researchers

- [Richard E. Carson](https://seas.yale.edu/faculty-research/faculty-directory/richard-e-carson)

Carson is one of the pioneers in PET kinetic modeling. He earned his PhD in UCLA, and he must has some relationships with Michael Phelps or Edward Hoffman. Now he is leading a team in Yale.

- Yun Zhou

Now in United Imaging Healthcare, Dr. Zhou is known for developing the reverse equilibrium plot and multi-graphical analysis.

- [Richard Wahl](https://profiles.wustl.edu/en/persons/richard-wahl)

Richard Wahl is one of the leading PET researchers in the US, and is the head of the radiology department of WUSTL. He is also one of the leading researchers in dynamic PET.

- [Guobao Wang](https://wanglab.faculty.ucdavis.edu/)

Guobao Wang is in UCDavis and his recent research highlight is on EXPLORER PET. He has a code for direct reconstruction of dynamic PET, [DIRECT v0.1](https://sites.google.com/site/gbwangonline/code).
