# Merge rasters

```python
from osgeo import gdal
import subprocess
import os
import glob
import config

ROI_LIST = ["ROI1", "ROI2"]
no_data_value = 10

INPUT_PATH_TIFFS = []
for ROI in ROI_LIST:
    INPUT_PATH_TIFF = glob.glob("PATH*.tif")
    INPUT_PATH_TIFFS.extend(INPUT_PATH_TIFF)
OUTPUT_PATH_MERGED_TIFF = os.path.join(
  "PATH_OUTPUT.tif")

for input_path in INPUT_PATH_TIFFS:
    dataset = gdal.Open(input_path, gdal.GA_Update)
    if dataset is None:
        print(f"Could not open {input_path}")
    else:
        band = dataset.GetRasterBand(1)
        band.SetNoDataValue(no_data_value)
        band.FlushCache()
        band=None
        dataset = None

command = [
    "gdal_merge.py",
    "-n", str(no_data_value),
    "-a_nodata", str(no_data_value),
    "-o", OUTPUT_PATH_MERGED_TIFF,
    "-co", "COMPRESS=LZW"
] + INPUT_PATH_TIFFS

try:
    result = subprocess.run(command, capture_output=True, text=True, check=True)
    print("Output:\n", result.stdout)  # Print standard output
except subprocess.CalledProcessError as e:
    print("Error:\n", e.stderr)  # Print standard error
```

