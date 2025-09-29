# Python API

You can use the [Python API](https://jupytergis.readthedocs.io/en/latest/user_guide/python_api.html) to add raster and vector layers, edit
symbology, and filter data. Moreover you can visualize the map in the
notebook, and use other GIS libraries for further data analysis.

You can create a new project or open an existing one in the notebook.
Let's open the project we have been working on, create a new color
expression and add a new layer.

```{admonition} Hands on
:class: tip
- Open the Launcher and select a Notebook kernel.
- Import the library.
    ```python
    from jupytergis import GISDocument
    ```
- Open the project.
    ```python
    doc = GISDocument('<project_name.jGIS>')
    ```
- Create a color expression.
    ```python
    color_expr = {
        "circle-fill-color":"#303757",
        "circle-radius":3
    }
    ```
- Add a new vector layer.
    ```python
    bike_stations = doc.add_geojson_layer(
        path=(
        "https://public.opendatasoft.com/api/explore/v2.1/catalog/datasets"
        "/jcdecaux_bike_data/exports/geojson"
        ),
        name="Bike stations",
        color_expr=color_expr
    )
    ``` 
- Visualize the map.
    ```python
    doc
    ```

- If the newly added layer is at the bottom, bring it to the top.
