# Extract by mask

```python
command = [
    "gdalwarp",
    "-crop_to_cutline",
    "-cutline", <SHAPE_PATH>,
    <MOSAIC_PATH>,
    <CLIPPED_PATH>]
try:
    result = subprocess.run(command, capture_output=True, text=True, check=True)
    print("Output (EXTRACT BY MASK):\n", result.stdout)
except subprocess.CalledProcessError as e:
    print("Error (EXTRACT BY MASK):\n", e.stderr)

```

