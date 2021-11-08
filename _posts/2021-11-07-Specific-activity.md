---
title: "Specific activity of carrier-free radionulicdes"
categories:
  - Imaging
tags:
  - study note
date: 2021-11-07
layout: post
---

Lately, several different collaborators thought about incorporating molecular imaging into their research. Without too much experience in radiolabeling technique, they turned to me for advise. However, one of the central concerns that they share, is "how much amount of atoms are in the specific dose of radioactive materials". Wether in moles or in grams, they are always eager to at least get some number to grasp upon.

Much as I doubt their real necessity, I perfectly understand the eagerness of their pursuit. When I first entered this trade, that was also my deep concern, how much atoms do we have their.

In this post, I am going to first explain the concept of specific activity, and how is it calculated. Then I will explain why it is not a prime concern in actual radiolabeling.

## Introducing the specific activity

The first concept to be introduced is carrier. The definition of carrier is herein cited from the [*International Pharmacopoeia*](https://www.who.int/medicines/publications/pharmacopoeia/Radgenmono.pdf) edited by WHO.

> **Carrier** The carrier, in the form of inactive material, either isotopic with the radionuclide, or non-isotopic, but chemically similar to the radionuclide, may be added during processing and dispensing of a radiopharmaceutical preparation to permit ready handling. In some situations, it will be necessary to add carrier to enhance chemical, physical or biological properties of the radiopharmaceutical preparation. The amount of carrier added must be sufficiently small for it not to cause undesirable physiological effects. The mass of an element formed in a nuclear reaction may be exceeded by that of the inactive isotope present in the target material or in the reagents used in the separation procedure.

As we can see, the carrier possesses two key characteristics. It is chemically close enough (or equivalent to) the radionuclide that we desire. However, it does not contribute to the radioactivity.

Carrier is an important concept to understand the specific activity. When the carrier is present, some of the amount of the delivered radionuclide does not contribute to the radioactivity. If the delivered radionuclide is **carrier-free**, or isotopic pure, the **specific activity** can be calculated according to the radioactive law. The specific activity, is defined in the same document as "the radioactivity per unit mass of the element or of the compound concerned".

Some of the decay processes transform the atom or labeled compound to its carrier, while other processes would not. For example, $\mathrm{^{99m}Tc\rightarrow ^{99}Tc}$ obviously create its carrier. However, the decay product of the $\mathrm{^{32}P \rightarrow ^{32}S}$ process is likely to be removed.

Now, let us suppose we are now at a specific time point when the separation is done and the product is carrier free, and we can answer the question that my collaborators ask.

First, we have the basic law of radioactivity, or the definition of radioactivity $A$.

$$A = -\frac{\mathrm{d}N}{\mathrm{d}t}=\lambda N$$

This equation relates the radioactivity $A$ to the decay constant $\lambda$ and the number of particles $N$. Here we also have

$$T_{1/2} = \frac{\ln 2}{\lambda}$$

and

$$n = \frac{N}{N_A}$$

$$w = n \cdot M_w$$

where $T_\frac{1}{2}$ corresponds to the half life, $N_A$ is the Avogadro constant, $w$ is the mass of the radioactive material, and $M_w$ is the molar mass of the corresponding material.

Taking everything together, we obtain the following relationship,

$$n = \frac{A}{\lambda N_A} = \frac{A \cdot T_\frac{1}{2}}{\ln 2\cdot N_A}$$

$$w = \frac{A \cdot T_\frac{1}{2} \cdot M_w}{\ln 2\cdot N_A}$$

and the specific activity $m$ could be calculated as

$$m = \frac{A}{w} = \frac{\ln 2 \cdot N_A}{T_\frac{1}{2}\cdot M_W}$$

## A Javascript-based tool and an example

As we can see, for a carrier-free radionuclide, once we know the half-life and the molar mass, we can calculate the amount of the radionuclide at a given level of radioactivity. I have written a [webpage-based application](https://mengxiangxi.info/Tools/Bq2nmol.html) to facilitate the calculations. It supports vaious input and units.

You can use a previously defined list of radionuclides (I might add more when I encounter them), or you can specify your own raidonulides. Let us suppose there is 5 mCi of carrier-free [<sup>18</sup>F]FDG, which is roughly the amount to be injected to a typical patient. The molecular weight of the labeled compound is 181.1496 g/mol. And we get the amount of [<sup>18</sup>F]FDG injected is 0.53 nmol, or 95.8 ng. The specific activity is 9.46 mCi/ng.

Take the Bayer® Aspirin for example. A typical tablet of Aspirin contains 325 mg of acetylsalicylate, which is 1.80 mmol. A tablet of Aspirin contains 3,396,226 times the molar amount of the [<sup>18</sup>F]FDG, or 3,392,484 times the mass.

## Conclusion

As I stated at the beginning, although specific activity is an important concept, it is not often the prime concern in the radiolabeling process of biomacromolecules or nanomaterials, as long as the radionuclide provided is carrier-free.

In the actual radiolabeling process, the labeling agent is in vast majority and the the amount of isotope is only minimum. A 10-fold change of radioactivity concentration makes little difference to the labeling chemistry. Meanwhile, we expect the biomacromolecule or nanomarterial to provide an ample amount of conjugation or chelation sites, so as to drive the reaction into full completion.
