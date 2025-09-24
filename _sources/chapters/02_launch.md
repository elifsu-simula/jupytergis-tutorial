# Launching JupyterGIS

We are going to access JupyterGIS from Galaxy's interactive tools.

:::{admonition} Hands on
:class: tip
- Open your browser and navigate to [https://usegalaxy.eu/join-training/bids25-jupytergis](https://usegalaxy.eu/join-training/bids25-jupytergis) to access the JupyterGIS platform.
- On the left sidebar, click Tools.
- In the search box, type JupyterGIS.
- Click Interactive JupyterGIS Notebook and then click the Run Tool button.
```{figure} ../images/run_tool.png
:name: run-tool
:width: 100%
Run tool
```
- Click on Interactive Tools on the left sidebar.
- Wait a few minutes until the job status changes to “running.”
- Click on the Jupyter Interactive GIS Tool link that appears to open JupyterLab.
```{figure} ../images/start_jgis.png
:name: start-jgis
:width: 100%
Start JupyterGIS
```
- Here you will see the Launcher. In the launcher you can select which kernel you want to use for a Jupyter notebook or Python console, or you can see the applications you can open. Here you can find the JupyterGIS application.Scroll down to the Other section and click GIS File to open a blank canvas for your project.
```{figure} ../images/new_gis_file.png
:name: new-gis-file
:width: 100%
New GIS File
```
:::

## Overview

Let's get familiar with the JupyterGIS file. On top you can find
the toolbar, where you can undo/redo your changes, open console, open
layer browser, add layers, center on current location, identify
features, and control temporal features.

On the left side you can find the list of layers added to the project,
SpatioTemporal Asset Catalogs (STAC) browser and filters. On the right
side you can find more information about the layer, such as its source,
symbology, annotations, and feature information. On the bottom you can
see the JupyterGIS version, the coordinates of the cursor, the scale,
the projection, and units.

```{figure} ../images/empty_project.png
:name: fig-empty-project
:width: 100%
Empty project
```
