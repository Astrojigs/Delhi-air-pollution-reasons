# Delhi-air-pollution-reasons
Finding parameters which are the main contributors to cause a rise in Delhi air pollution using QGIS and SQL (PostgreSQL)

# Motivation:

1. Worsening situations leading to unbreathable air in Delhi.

2. Following regions of interests:
- Whether agricultural harvesting in close-by regions affect the overall air quality in Delhi.
- Does Wind direction play an important role in air quality?
- Which month do we see maximum amount of air pollution in Delhi?

# Analysis:
## Data Required:
*Give links to all the Data*
- Fire data
- Climate, wind Data
- Air quality index and particle pollutants measurements Data
- Annual pollution data

## Visualizing Fire Data:
Using GIS software like QGIS we can plot out the fire data from 2019.

![Fire data](https://github.com/Astrojigs/Delhi-air-pollution-reasons/blob/main/Photos/All%20Fire%20data%20points.png)

Each red marker indicates an incident involving fire has taken place.

### Creating a view using SQL:
plotting points around Delhi within a radius of 500 kilometers.

```
drop view if exists fire_data_2019_view;
create or replace view fire_data_2019_view;
select ogc_field, wkb_geometry, latitude, longitude, brightness, acq_date
from fire_data_2019 view where
st_distance(st_transform(ST_GEOMFromText('POINT(77.1890 28.5120)', 4326), 7755),
            st_transform(wkb_geometry,7755)) < 500*1000
```
*Output*

![Fire data 2019 view](https://github.com/Astrojigs/Delhi-air-pollution-reasons/blob/main/Photos/fire%20data%202019%20view.png)

### Identifying Types of Fire using gradients:
Since fire can come in all forms of shapes and sizes. It is best to take into account the intensity of these fires. Higher intensity implies more pollution in nearby region.

![Graduated symbology fire data](https://github.com/Astrojigs/Delhi-air-pollution-reasons/blob/main/Photos/graduated%20symbology%20fire%20data%202019.png)
