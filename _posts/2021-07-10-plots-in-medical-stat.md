---
title: "Statistical plots in clinical studies"
categories:
  - Statistics
tags:
  - study note
  - checklist
date: 2021-07-10
layout: post
---

## ROC plot

The receiver operating plot is used in determining the performance of a discrete model.

![ROC curve](/assets/PlotsInMedicalStat/ROC.png)

The ROC plot

```python
sklearn.metrics.roc_curve(y_true, y_score, *, pos_label=None, sample_weight=None, drop_intermediate=True)
```


## Bland-Altman plot

![Bland-Altman plot](/assets/PlotsInMedicalStat/BlandAltmanROC.png)

## Nomogram

## Forest plot
