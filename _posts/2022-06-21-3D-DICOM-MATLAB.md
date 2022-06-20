---
title: "Notes on tags of UIH 3D DICOM in MATLAB"
categories:
- Imaging
tags:
- cheat sheet
- dynamic imaging
date: 2022-06-21
layout: post
---

In MATLAB, the processing of DICOM files is taken care of by the [Image Processing Toolbox](https://www.mathworks.com/help/images/dicom-support-in-the-image-processing-toolbox.html). However, as far as I know, this toolbox does not support metadata indexing via numerical tags. In 3D DICOM by developers of United Imaging uEXPLORER, due to the hierarchy structure, indexing through tag names is very inconvenient.

Here, I am keeping a list of the relationship between the 2D DICOM tag and the corresponding 3D DICOM tag. I hope it will help people working with 3D DICOM files on MATLAB.

## Basic

- Rescale Slope

| | |
| --- | --- |
| Tag | 0028, 1053 |
| 2D DCM | RescaleSlope |
| 3D DCM | SharedFunctionalGroupsSequence.Item_1.PixelValueTransformationSequence.Item_1.RescaleSlope |

## Radionuclide properties

- Radiopharmaceutical half life

| | |
| --- | --- |
| Tag | 0018, 1075 |
| 2D DCM | RadiopharmaceuticalInformationSequence.Item_1.RadionuclideHalfLife |
| 3D DCM | RadiopharmaceuticalInformationSequence.Item_1.RadionuclideHalfLife |

- Radionuclide total dose

The injection dose at the Radiopharmaceutical Start Time

| | |
| --- | --- |
| Tag | 0018, 1074 |
| 2D DCM | RadiopharmaceuticalInformationSequence.Item_1.RadionuclideTotalDose |
| 3D DCM | RadiopharmaceuticalInformationSequence.Item_1.RadionuclideTotalDose |

- Radiopharmaceutical start time

| | |
| --- | --- |
| Tag | 0018, 1072 |
| 2D DCM |  RadiopharmaceuticalInformationSequence.Item_1.RadiopharmaceuticalStartTime |
| 3D DCM |  RadiopharmaceuticalInformationSequence.Item_1.RadiopharmaceuticalStartTime |

## Decay correction

- Decay correction

The time point of decay correction ("NO", "START", "ADMIN")

| | |
| --- | --- |
| Tag | 0054, 1102 |
| 2D DCM |  DecayCorrection |
| 3D DCM |  PerFrameFunctionalGroupsSequence.Item_1.PETFrameCorrectionFactorsSequence.Item_1.DecayCorrection |

## 2D DCM or 3D DCM?

I use this syntax to determine whether the dcm file read is 2D or 3D.

```matlab
info = dicominfo("dicomfile.dcm")

try info.SliceLocation;
    disp("Reading 2D DICOM files.")
catch
    disp("Reading 3D DICOM files.")
end
```
