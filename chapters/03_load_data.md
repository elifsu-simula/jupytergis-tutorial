# Loading and Visualizing Geospatial Data

## Adding Layers and Editing Symbology

Let's start with adding a base raster layer.

:::{admonition} Hands on
:class: tip
- Click on the Layer Browser button, then select OpenStreetMap.Mapnik.
- Try panning and zooming in/out using your mouse.
- Try the Center on Geolocation button while zoomed in.
:::

Notice that the layer we added now appears in the layers tab on the left
side. If you click on it you can find more information on the object
properties on the right side, such as the source URL.

:::{admonition} Hands on
:class: tip
- Hide the layer by clicking on the eye button on the left panel.
- Change the opacity of the layer.
:::

Now let's add a vector layer on top of our raster layer.

**Hands on:**
:::{admonition} Hands on
:class: tip
- Click on +, Add Vector Layer, New Shapefile Layer.
- Enter the following URL:
  <https://public.opendatasoft.com/api/explore/v2.1/catalog/datasets/world-administrative-boundaries-countries/exports/shp>
- Click Ok.
:::

Now you should see the world country boundaries on your map. Let's
rename the layer and change the symbology.

**Hands on:**
:::{admonition} Hands on
:class: tip
- Right click on the Custom Shapefile layer on the left panel.
- Select Rename Layer, then enter a new name, such as *Countries*.
- Right click again, and select Edit Symbology.
- Change the fill and stroke colors, and stroke width.
- Under processing, try convex hull, centroids, bounding boxes and
  concave hull.
- Change the order of the layers on the left panel and observe the
  changes.
:::

## STAC Browser

In the left panel, you can find the STAC Browser tab. You can zoom into
different parts of the map to set your area of interest, or tick the
"Use whole world as bounding box" button to disable spatial filtering.

:::{admonition} Hands on
:class: tip
- Explore the STAC browser.
- Add one layer from the STAC browser in an area of interest of your
  choice.
:::

## Save the Project

Right now the project has the default name untitled.jgis. Let's save it
with a better name.

:::{admonition} Hands on
:class: tip
- Click on File, Save JGIS As...
- Enter a descriptive name.
:::
