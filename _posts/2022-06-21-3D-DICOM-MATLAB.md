---
title: "Notes on tags of UIH 3D DICOM in MATLAB (and Python)"
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

> Updated 28th Feb, 2024: I am appending the Python version of the tag list. The Python library `pydicom` is used to read the DICOM files.
>
> ```python
> import pydicom
> 
> ds = pydicom.dcmread("dicomfile.dcm")
> tag1 = ds.TagName
> n = 0 # for the first item
> tag2 = ds.TagGroup[n].TagName
> ```
>
> Similarly, the MATLAB code for accessing the tag is like:
>
> ```matlab
> info = dicominfo("dicomfile.dcm")
> tag1 = info.TagName
> tag2 = info.TagGroup.Item_1.TagName % for the first item
> ```

## Basic

- Rescale Slope

| Item | Value |
| --- | --- |
| Tag | 0028, 1053 |
| 2D DCM | RescaleSlope |
| 3D DCM, MATLAB | SharedFunctionalGroupsSequence.Item_1.PixelValueTransformationSequence.Item_1.RescaleSlope |
| 3D DCM, python | SharedFunctionalGroupsSequence[0].PixelValueTransformationSequence[0].RescaleSlope |

- Pixel Spacing

| Item | Value |
| --- | --- |
| Tag | 0028, 0030 |
| 2D DCM | PixelSpacing |
| 3D DCM, MATLAB | SharedFunctionalGroupsSequence.Item_1.PixelMeasuresSequence.Item_1.PixelSpacing |
| 3D DCM, python | SharedFunctionalGroupsSequence[0].PixelMeasuresSequence[0].PixelSpacing |

- Slice Thickness

| Item | Value |
| --- | --- |
| Tag | 0018, 0050 |
| 2D DCM | SliceThickness |
| 3D DCM, MATLAB | SharedFunctionalGroupsSequence.Item_1.PixelMeasuresSequence.Item_1.SliceThickness |
| 3D DCM, python | SharedFunctionalGroupsSequence[0].PixelMeasuresSequence[0].SliceThickness |

## Radionuclide properties

- Radiopharmaceutical half life

| Item | Value |
| --- | --- |
| Tag | 0018, 1075 |
| 2D DCM | RadiopharmaceuticalInformationSequence.Item_1.RadionuclideHalfLife |
| 3D DCM, MATLAB | RadiopharmaceuticalInformationSequence.Item_1.RadionuclideHalfLife |
| 3D DCM, python | RadiopharmaceuticalInformationSequence[0].RadionuclideHalfLife |

- Radionuclide total dose

The injection dose at the Radiopharmaceutical Start Time

| Item | Value |
| --- | --- |
| Tag | 0018, 1074 |
| 2D DCM | RadiopharmaceuticalInformationSequence.Item_1.RadionuclideTotalDose |
| 3D DCM, MATLAB | RadiopharmaceuticalInformationSequence.Item_1.RadionuclideTotalDose |
| 3D DCM, python | RadiopharmaceuticalInformationSequence[0].RadionuclideTotalDose |

- Radiopharmaceutical start time

| Item | Value |
| --- | --- |
| Tag | 0018, 1072 |
| 2D DCM |  RadiopharmaceuticalInformationSequence.Item_1.RadiopharmaceuticalStartTime |
| 3D DCM, MATLAB |  RadiopharmaceuticalInformationSequence.Item_1.RadiopharmaceuticalStartTime |
| 3D DCM, python |  RadiopharmaceuticalInformationSequence[0].RadiopharmaceuticalStartTime |

## Decay correction

- Decay correction

The time point of decay correction ("NO", "START", "ADMIN")

| Item | Value |
| --- | --- |
| Tag | 0054, 1102 |
| 2D DCM |  DecayCorrection |
| 3D DCM, MATLAB |  PerFrameFunctionalGroupsSequence.Item_1.PETFrameCorrectionFactorsSequence.Item_1.DecayCorrection |
| 3D DCM, python |  PerFrameFunctionalGroupsSequence[0].PETFrameCorrectionFactorsSequence[0].DecayCorrection |

## More about 3D DICOM

### 2D DCM or 3D DCM?

I use this syntax in MATLAB to determine whether the dcm file read is 2D or 3D.

```matlab
info = dicominfo("dicomfile.dcm")

try info.SliceLocation;
    disp("Reading 2D DICOM files.")
catch
    disp("Reading 3D DICOM files.")
end
```

### PerFrameFunctionalGroupsSequence

The `PerFrameFunctionalGroupsSequence` is a collection of individual metadata which would otherwise be in each slice (2D DICOM file). It has multiple `Item`s, and the `i`'th slice is represented in `PerFrameFunctionalGroupsSequence.Item_i`.

### How to find corresponding metadata in MATLAB

```MATLAB
% First, obtain the DICOM information struct
info = dicominfo("#PATH_TO_DCM_FILE#");

% Direct the output to a text file
diary('./dcminfo.txt')

% Write the struct into the text file
% Use the third-party function `unfold()`
unfold(info)

% Search for the keyword using a text editor

% Stop recording the output
diary off
```
