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

