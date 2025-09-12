# Filtering

If you check out the data
[here](https://public.opendatasoft.com/explore/dataset/jcdecaux_bike_data/table/),
you can see that it has many features. Let's identify our data and
filter out the closed stations from our map.

:::{admonition} Hands on
:class: tip
- Open the JupyterGIS project, and open the Identify Features tab on the
  right panel.
- Click on Identify tool on the toolbox.
- Select Countries layer on the left panel and click on a country.
  Observe the features on the right panel.
- Click on the Bike stations layer on the left panel and click on a data
  point. If you have selected multiple data points, try zooming in and
  clicking again. Observe the features on the right panel.
- Go back to the notebook and filter the bike stations based on the
  status feature.

  ``` python
  doc.add_filter(
      layer_id=bike_stations,
      logical_op="all",
      feature="status",
      operator="==",
      value="OPEN"
  )
  ```

- Check out the map. Are the closed bike stations still on the map?
:::

Let's also filter the bike stations where there are more than 10 bike
stands using the GUI.

:::{admonition} Hands on
:class: tip
- On the left panel, click on Filters.
- Click Add, then select Bike_stands, \>, 10.
- Click Submit.
- If you change All to Any, what happens? What effect does it have?
:::