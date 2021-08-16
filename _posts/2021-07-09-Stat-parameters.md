---
title: "Checklist of some statistical parameters"
categories:
  - Statistics
tags:
  - checklist
  - cheat sheet
date: 2021-07-09
layout: post
---

> There are various statistical parameters, especially metrics, that are rather confounding. Here is a list as a quick reference.

## Evaluation metrics of a binary predictive model

- Sensitivity, a.k.a., Recall, True positive rate (TPR)

$$\mathrm{Sensitivity} = \frac{\mathrm{TP}}{\mathrm{TP}+\mathrm{FN}}$$

- Specificity

$$\mathrm{Specificity} = \frac{\mathrm{TN}}{\mathrm{TN}+\mathrm{FP}}$$

- False positive rate, FPR

$$\mathrm{FPR} = \frac{\mathrm{FP}}{\mathrm{TN}+\mathrm{FP}} = 1 - \mathrm{Specificity}$$

- Positive predict value, PPV; a.k.a., Precision

$$\mathrm{PPV} = \frac{\mathrm{TP}}{\mathrm{TP}+\mathrm{FP}}$$

- Negative predict value, NPV

$$\mathrm{NPV} = \frac{\mathrm{TN}}{\mathrm{TN}+\mathrm{FN}}$$

- Likelihood radios, LR+, LR-

$$\mathrm{LR}^+ = \frac{\mathrm{Sensitivity}}{1-\mathrm{Specificity}} = \frac{\mathrm{TP}(\mathrm{TN}+\mathrm{FP})}{\mathrm{FP}(\mathrm{TP}+\mathrm{FN})}$$

$$\mathrm{LR}^- = \frac{1-\mathrm{Sensitivity}}{\mathrm{Specificity}} = \frac{\mathrm{FN}(\mathrm{TN}+\mathrm{FP})}{\mathrm{TN}(\mathrm{TP}+\mathrm{FN})}$$

- Accuracy

$$\mathrm{Accuracy} = \frac{\mathrm{TP}+\mathrm{TN}}{\mathrm{P}+\mathrm{N}}$$

- Precision ==> PPV

$$\mathrm{Precision} = \frac{\mathrm{TP}}{\mathrm{TP}+\mathrm{FP}}$$

- Recall ==> Sensitivity

$$\mathrm{Recall} = \frac{\mathrm{TP}}{\mathrm{TP}+\mathrm{FN}}$$

- $F_1$ score, the harmonic mean of Precision and Recall.

$$
\begin{aligned}
F_1 &= 2\times\frac{\mathrm{Precision}\times\mathrm{Recall}}{\mathrm{Precision}+\mathrm{Recall}}\\
& = \frac{2\times\mathrm{TP}}{2\times\mathrm{TP}+\mathrm{FP}+\mathrm{FN}}
\end{aligned}
$$
