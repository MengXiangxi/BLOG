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

Plot analysis is a unique feature in medical statistics, and is widely used in all aspects of data science. Here, several types of plot analysis are summarized, including the appearance, utility and implementations.

## ROC plot

The receiver operating plot is used in determining the performance of a discrete model. The horizontal axis is FPR, or 1-Specificity; the vertical axis is TPR, or Sensitivity.

![ROC curve]({{ site.baseurl }}/assets/PlotsInMedicalStat/ROC.png)

The area under curve (AUC) is calculated through integrating the area under the ROC curve. The curve could be calculated using `scikit-learn`.

```python
sklearn.metrics.roc_curve(y_true, y_score, *, pos_label=None, sample_weight=None, drop_intermediate=True)
```

To statistically test whether two ROC are significantly different, the Delong's test is often used. I have adapted a code for that.

## Bland-Altman plot

This plot is used for showing the agreement between two models on the same input. All inputs are run through the two methods, and the outputs are paired. Then the difference between each pair is plotted against their average. Simple.

![Bland-Altman plot]({{ site.baseurl }}/assets/PlotsInMedicalStat/BlandAltman.png)

Bland-Altman can be conveniently generated by many types of software. OriginPro even provides a template for it.

## QQ plot

To compare the sample distribution with the theoretical one, the QQ plot could be used.

![Bland-Altman plot]({{ site.baseurl }}/assets/PlotsInMedicalStat/QQplot.png)

The QQ plot can be generated by python using `scipy.stats.probplot()` in `scipy`, or `statsmodels.graphics.gofplots.qqplot` in `statsmodel`.

## Nomogram

Nomogram is a somewhat complicated analysis technique in medical statistics, it is associated with multivariate regression analysis, often generalized linear model.

![Bland-Altman plot]({{ site.baseurl }}/assets/PlotsInMedicalStat/nomogram.png)

Although there is a python package (`PyNomo`) to generate virtually any type of nomogram, the most convenient practice is to use R (`rms`).

## Forest plot

Forest plot is often associated with meta analysis and systematic reviews. However, it can be used to compare the risk factors in many different scenarios.

![Bland-Altman plot]({{ site.baseurl }}/assets/PlotsInMedicalStat/forest.png)

In python, `statsmodels` can be used to conduct meta analysis and draw forest plot. However, the appearance is kind of strange. The `forestplot` seems to be more convenient. OriginPro has a custom template file [here](https://www.originlab.com/fileExchange/details.aspx?fid=362).