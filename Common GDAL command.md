## Common GDAL command

## Run GDAL command by python script

`subprocess.check_output` is a Python function that runs an external command and returns its output. If the command exits with a non-zero status (which indicates an error), it raises a `CalledProcessError`. By capturing the output, you can see what the command did, which is particularly useful for debugging or logging.

```python
# RECOMMENDED USAGE
import subprocess
command = [
    "gdal_rasterize", # example: vector to raster
    "-ts", str(NUM_WIDTH), str(NUM_HEIGHT),
    "-a", RASTER_VALUE_ATTRIBUTE_NAME,
    "-of", "GTiff",
    "-ot", "DATA_TYPE", # e.g., "Byte"
    "-co", COMPRESSION_TYPE, # e.g., "COMPRESS=LZW",
    INPUT_PATH_SHAPEFILES,
    OUTPUT_PATH_RASTER
]

try:
    result = subprocess.run(command, capture_output=True, text=True, check=True)
    print("Output:\n", result.stdout)
except subprocess.CalledProcessError as e:
    print("Error:\n", e.stderr)
```

<u>*ATTENTION: Using notebook with CLI often results in errors*</u>

- <u>*It works for Linux*</u>

- <u>*It works on anaconda prompt and jupyter-notebook (not Colab) on windows*</u>

```shell
!gdaltindex "PATH_OUTPUT_SHAPE" "PATH_REF_TIFF"
```

- <u>*It works on  jupyter-notebook (not Colab) on windows*</u>

```python
subprocess.run(["gdaltindex", PATH_GENERATED_SHAPE, PATH_REF_TIFF], capture_output=True, text=True, check=True, shell=True)
```

## Common GDAL command

```python
# merge images
gdal_merge.py -o {OUTPUT_PATH_MERGED_TIFF} -co {COMPRESSION_TYPE} {INPUT_PATH_FILES}
'''
command = [
    "gdal_merge.py",
    '-o', mosaic_path,
    '-co', 'COMPRESS=LZW',
    ]+config.TILES_TRAIN_BEN
'''

# from raster to boundary shapefile
gdaltindex {OUTPUT_PATH_SHAPEFILE} {INPUT_PATH_TIFF}

# resampling
gdalwarp -crop_to_cutline -cutline {PATH_BOUNDARY} -ts {NUM_WIDTH} {NUM_HEIGHT} {INPUT_PATH_TIFF} {OUTPUT_PATH_RESAMPLED_TIFF}

# vector data rasterization
gdal_rastersize -ts {NUM_WIDTH} {NUM_HEIGHT} -a {RASTER_VALUE_ATTRIBUTE_NAME} -of GTiff -ot {DATA_TYPE} -co {COMPRESSION_TYPE} {INPUT_PATH_SHAPEFILES} {OUTPUT_PATH_RASTER}

# clip imagery with boundary shapefile
gdalwarp -overwrite -dstnodata {UNKNOW_CLASS} -crop_to_cutline -cutline {SHAPE_PATH} {MERGED_PATH} {CLIPPED_PATH}
```

