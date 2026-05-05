# CONVERT NRRD AND SEGMENTATION NRRD FILES TO ADF FILES

# USAGE

## GUI:
Run the GUI using the following command in a terminal
```bash
python3 nrrd_to_adf_gui.py
```
<p style="text-align: center"><img src="media/gui.png" alt="GUI" width="500"/></p>

## Command line:
Run the Python script with the command line options

```bash
python3 nrrd_to_adf.py -n volume.nrrd -a volume.yaml
```

Check the available options via:

```bash
python3 nrrd_to_adf.py -h
```


|#    |Option   |Description   |
|-----|---------|--------------|
|-h   |Help     | Show help    |
|-n   |NRRD or Seg NRRD filepath| Provide the *.nrrd or *.seg.nrrd filepath to be loaded|
|-a   |ADF filepath| Provide the *.yaml AMBF Description filepath to be saved|
|-p   |Optional slices prefix| Optional prefix to be appended to saved slices|
|--slices_path   |Optional Path for slices| Provide a path for the slices|

## Re-importing PNGS to slicer 3D 

Tested on Slicer `5.8.1` and Ubuntu 24.04

### Option 1: Convert back to .nrrd
Use `update_seg_nrrd.py` to convert the PNG slices back to a .seg.nrrd file. 

```bash
python update_seg_nrrd.py \
    -n data/testing_data_cynthia/seg_post.seg.nrrd \
    -s data/testing_data_cynthia/seg_post.seg_slices \
    -p slice0 \
    -o seg_post_updated.seg.nrrd
```

### Option 2: Re-importing PNGs directly
In Slicer 3D `5,8.1`, the PNGs exported by `seg_nrrd_to_pngs.py` can be re-imported as a volume. However, the PNGs need to be flipped and converted to RGBA format. This can be done using ImageMagick with the following command:

```bash
mkdir -p png-fixed-1mm

for f in ./volume_with_1mm_drill/*.png; do
  convert "$f" \
    -flip \
    -alpha on \
    -define png:color-type=6 \
    -depth 8 \
    "png-fixed-1mm/$(basename "$f")"
done

```
