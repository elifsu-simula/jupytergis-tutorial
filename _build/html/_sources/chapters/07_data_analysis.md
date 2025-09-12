# Data Analysis: NDVI in Oslo

In this part of the tutorial, we are going to calculate the Normalized
Difference Vegetation Index (NDVI) for Oslo, Norway in the 1980s and
2020s, then compare them to find out where vegetation cover has
increased or decreased. NDVI is calculated using red (RED) and
near-infrared (NIR) bands with the following formula:

$$NDVI = \frac{NIR - RED}{NIR + RED}$$

We are going to use two satellite datasets:

- Landsat 5 Thematic Mapper, 1985--1986

- Copernicus Sentinel-2 L2A, 2020--2023


:::{admonition} Hands on
:class: tip
- Open the Launcher and select a Notebook kernel.

  ``` python
  # Install packages
  ! pip install -q pystac-client shapely planetary_computer stackstac matplotlib xarray rioxarray

  # Imports
  from pystac_client import Client
  from shapely.geometry import box, mapping
  import planetary_computer as pc
  from pyproj import Transformer
  import stackstac
  import matplotlib.pyplot as plt
  import xarray as xr
  import numpy as np
  import rioxarray
  ```

- Define constants.

  ``` python
  oslo_bbox = (10.40, 59.75, 10.95, 60.10)

  era_80s = "1985-06-01/1986-09-30"
  era_20s = "2020-06-01/2023-09-30"

  cloud_cover_threshold = 10

  pc_stac = "https://planetarycomputer.microsoft.com/api/stac/v1"
  collection = "landsat-c2-l2"

  output_epsg = 32632
  output_resolution_m = 60

  assets = ['red', 'nir08']
  ```

- Search Planetary Computer for STAC datasets.

  ``` python
  cat = Client.open(pc_stac)

  # Search for 80s data
  search = cat.search(
      collections=[collection],
      intersects=mapping(box(*oslo_bbox)),
      datetime=era_80s,
      query={"platform": {"eq": "landsat-5"}, "eo:cloud_cover": {"lt": cloud_cover_threshold}},
      limit=200,
  )
  items_80s = [pc.sign(item) for item in search.get_items()]

  # Search for 20s data
  search = cat.search(
      collections=[collection],
      intersects=mapping(box(*oslo_bbox)),
      datetime=era_20s,
      query={"platform": {"in": ["landsat-8","landsat-9"]}, "eo:cloud_cover": {"lt": cloud_cover_threshold}},
      limit=200,
  )
  items_20s = [pc.sign(item) for item in search.get_items()]

  print(f'Found {len(items_80s)} items for 1980s')
  print(f'Found {len(items_20s)} items for 2020s')
  ```

- Calculate Oslo bounds in UTM coordinates.

  ``` python
  t = Transformer.from_crs(4326, output_epsg, always_xy=True)
  xmin, ymin = t.transform(oslo_bbox[0], oslo_bbox[1])
  xmax, ymax = t.transform(oslo_bbox[2], oslo_bbox[3])

  oslo_bounds_utm = (xmin, ymin, xmax, ymax)     
  ```

- Stack datasets onto a common 60 m grid for each era.

  ``` python
  stack_80s = stackstac.stack(items_80s, assets=assets, epsg=output_epsg, resolution=output_resolution_m, bounds=oslo_bounds_utm, dtype="float", fill_value=0, chunksize=2048)
  stack_80s = stack_80s.groupby("time").mean("time", skipna=True)

  stack_20s = stackstac.stack(items_20s, assets=assets, epsg=output_epsg, resolution=output_resolution_m, bounds=oslo_bounds_utm, dtype="float", fill_value=0, chunksize=2048)
  stack_20s = stack_20s.groupby("time").mean("time", skipna=True)
  ```

- Compute NDVI (median composite per era).

  ``` python
  red80 = stack_80s.sel(band='red').astype("float32") / 10000.0
  nir80 = stack_80s.sel(band='nir08').astype("float32") / 10000.0
  ndvi80 = ((nir80 - red80) / (nir80 + red80)).clip(-1, 1)
  ndvi80_med = ndvi80.median("time", skipna=True).rename("NDVI_1980s_median")

  red20 = stack_20s.sel(band='red').astype("float32") / 10000.0
  nir20 = stack_20s.sel(band='nir08').astype("float32") / 10000.0
  ndvi20 = ((nir20 - red20) / (nir20 + red20)).clip(-1, 1)
  ndvi20_med = ndvi20.median("time", skipna=True).rename("NDVI_2020s_median")
  ```

- Plot side-by-side and NDVI difference.

  ``` python
  fig, axes = plt.subplots(1, 2, figsize=(12,6))
  ndvi80_med.plot.imshow(ax=axes[0], vmin=0, vmax=1, robust=True, cmap="YlGn")
  axes[0].set_title("NDVI in 1980s")
  ndvi20_med.plot.imshow(ax=axes[1], vmin=0, vmax=1, robust=True, cmap="YlGn")
  axes[1].set_title("NDVI in 2020s")
  plt.tight_layout(); plt.show()

  ndvi20a, ndvi80a = xr.align(ndvi20_med, ndvi80_med, join="inner")
  diff = (ndvi20a - ndvi80a).where(np.isfinite(ndvi20a + ndvi80a)).rename("NDVI_change_2020s_minus_1980s")

  plt.figure(figsize=(7,6))
  diff.plot.imshow(robust=True, cmap="RdBu")
  plt.title("NDVI change")
  plt.tight_layout(); plt.show()
  ```

- Export GeoTIFFs.

  ``` python
  ndvi80_r = ndvi80_med.rio.write_crs(output_epsg)
  ndvi20_r = ndvi20_med.rio.write_crs(output_epsg)
  diff_r = diff.rio.write_crs(output_epsg)

  ndvi80_r.rio.to_raster("oslo_ndvi_1980s.tif")
  ndvi20_r.rio.to_raster("oslo_ndvi_2020s.tif")
  diff_r.rio.to_raster("oslo_ndvi_change.tif")
  ```

- Now open your project, then add the exported GeoTiff layers to your
  project. Click on +, Add Raster Layer, New GeoTiff Layer.
