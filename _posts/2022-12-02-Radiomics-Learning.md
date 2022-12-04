---
title: "A learning primer to Radiomics"
categories:
- Imaging
tags:
- study note
date: 2022-12-02
layout: post
---

Radiomics is a technique in medical image analysis. It is about extracting and analyzing features in medical images. In this post, I am going to provide a brief note on the basic aspects of radiomics, providing some learning materials.

## Prerequisite

- Radiomics often involves (classical) statistical learning and/or deep learning. You need to master the basic ideas in machine learning if you want to conduct research in this area. Accordingly, a minimum level of statistics is also required.
- You need to know something about the DICOM standard, so as to process real-world medical imaging data. You may want to pick your favorite [DICOM viewer](https://mengxiangxi.info/Blog/medicalImagingSoftware.html).
- You need to master python. It would be helpful if you are familiar with the following packages, although you can learn them whenever necessary:
	- `numpy`, `pandas`, for basic data manipulation;
	- `matplotlib`, `simpleITK`, `opencv-python` (`cv2`), for image processing;
	- `pydicom`, for DICOM data import;
	- `torch`, `scikit-learn` and any other machine learning packages.

## Historical background and personal remarks

The name radiomics is obviously inspired by the 'omics' era in the early 2000's, and in fact, it was first introduced in [a 2012 paper by Philippe Lambin](https://linkinghub.elsevier.com/retrieve/pii/S0959804911009993). It is a very bright idea, yet in retrospect, it is actually very intuitive (like many other great ideas). The foundations of radiomics root deeply in decades of discoveries in digital image processing and representation learning.

Although radiomics introduce a whole set of pipeline in image data processing, the core concept lies in the feature extraction operation. In this process, standardization and harmonization are vital to the feature stability and generalization ability.

## Learning materials

There are many materials for introductory level radiomics learning. I am not providing a complete list of the materials for you to study, rather, I am only showing you what kinds of  materials could be beneficial in your learning.

- Review articles are published on prestigious journals, which can be a great primer. Here are just a few review paper that I have read.
	- The 2021 review [Radiomics in Oncology: A Practical Guide](https://pubs.rsna.org/doi/10.1148/rg.2021210037) published on RadioGraphics briefly establishes the concept and applications of radiomics.
	- Another review article to [Radiomics in medical imaging—“how-to” guide and critical reflection](https://insightsimaging.springeropen.com/articles/10.1186/s13244-020-00887-2)
	- A review in IEEE Signal Processing Magazine [From Handcrafted to Deep-Learning-Based Cancer Radiomics: Challenges and Opportunities](https://ieeexplore.ieee.org/document/8746872) provides a comprehensive account of radiomics.
- Books, monographs and textbooks.
	- There are not too many books out there. I find one of them pretty interesting, which was edited by Dr. Jie Tian and collaborators. The title is [Radiomics and Its Clinical Application](https://www.elsevier.com/books/radiomics-and-its-clinical-application/tian/978-0-12-818101-0). In this book, they introduced their experiences in radiomics study, including the basic pipeline (Chapter 2) and a literature survey (Chapters 3 and 4).
- Online courses
	- Have not found any good one yet.

## Software and platforms

- The most important tool in radiomics study is `pyradiomics`, whose documentations are [here](https://pyradiomics.readthedocs.io/en/latest/). For the most part, `pyradiomics` is compliant with the IBSI standard (see below), and is one of the most popular radiomics tool used by the scientific community. Using `pyradiomics`, you can customize your own working pipelines. This is what we do in our research.
- [LIFEx](https://www.lifexsoft.org/) is a well-known radiomics tool. It has a graphic interface and is rather easy to use. However, it provides relatively small flexibility to customize.
- The [Radiomics](http://www.radiomics.net.cn/platform/index) software developed by Dr. Jie Tian.

## Other resources

- As I have stated, standardization and harmonization are two key issues in radiomics. Without them, the generalization ability of radiomic models are questionable. The [Imaging Biomarker Standardization Initiative](https://theibsi.github.io/) is an organization devoted to develop standardized image biomarkers (features) for radiomics analysis. The works of this collaboration are highly valued by the community, and the collaboration is still continuing.
- You are encouraged to explore the GitHub topic page of [#radiomics](https://github.com/topics/radiomics) for related code and projects.