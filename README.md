# Different methods to load/save the CT images of ANALYZE 7.5 format in Matlab 

## Prerequisite files
* An ANALYZE image database
  * An image file (*.img*)
  * A header file (*.hdr*)
* Corresponding 3rd party packages

(an image file *.img* and a header file *.hdr*)

## Methods to load the images
1. Load image with [**Read Medical Data 3D**](https://www.mathworks.com/matlabcentral/fileexchange/29344-read-medical-data-3d) by Dirk-Jan Kroon

```
% Add package to path
addpath('ReadData3D_version1k');

% Read image file
[V,info]=ReadData3D('21405_3a_ClippedProp.hdr');

% Access the CT volume
[dx, dy, dz] = size(V);
V(122, 122, 122) = 200;

```

2. Load image with [**Tools for NIfTI and ANALYZE image**](https://www.mathworks.com/matlabcentral/fileexchange/8797-tools-for-nifti-and-analyze-image) by Jimmy Shen

```
% Add package to path
addpath('NIfTI_AnalyzeRW_Tools');

% Read image file
nii = load_nii('21405_3a_ClippedPropOri.hdr');

% Access the CT volume
[dx, dy, dz] = size(nii.img);
nii.img(122, 122, 122) = 200;

```

3. Load image with Matlab build-in function [**analyze75read**](https://www.mathworks.com/help/images/ref/analyze75read.html)

```

% Read image file
info = analyze75info('21405_3a_ClippedPropOri.hdr');
V = analyze75read(info);

% Access the CT volume
[dx, dy, dz] = size(V);
V(122, 122, 122) = 200;

```

4. Load image with the **fileFilers/analyze** packages under [*VISTASOFT*](https://github.com/patdonnelly/vistalab) by the Vista lab at Stanford University

```
% Add package to path
addpath('analyze');

% Read image file, specify endianType and data type
[V, mmPerVox, hdr, filename, fullHeader] = loadAnalyze('21405_3a_ClippedPropOri.hdr', 'ieee-be', 'uint8');

% Access the CT volume
[dx, dy, dz] = size(V);
V(122, 122, 122) = 200;

```


## Methods to load the images

1. Save image with [**Tools for NIfTI and ANALYZE image**](https://www.mathworks.com/matlabcentral/fileexchange/8797-tools-for-nifti-and-analyze-image) by Jimmy Shen

```
% Save image in ANALYZE format, if read with the same package, use 'nii'
save_nii(nii, '21405_3a_ClippedProp.hdr');

% Save image in ANALYZE format from a volume 'V'
V2=uint8(V);
analyzeim=make_ana(V2);
save_untouch_nii(analyzeim,'21405_3a_ClippedProp.hdr');
```

2. Save image with the **fileFilers/analyze** packages under [*VISTASOFT*](https://github.com/patdonnelly/vistalab) by the Vista lab at Stanford University

```
% Save image in ANALYZE format, if read with the same package, reuse Delta information in 'mmPerVox'
hdr2 = saveAnalyze(V, '21405_3a_ClippedProp', mmPerVox);

% Save image in ANALYZE format from a volume 'V'
hdr2 = saveAnalyze(V, '21405_3a_ClippedProp');
```


