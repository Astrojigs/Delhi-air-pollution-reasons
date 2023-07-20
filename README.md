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

Using this we can identify key positions where fire is more prominent. Since the fire data available is for multiple years,
We can use temporal analysis using QGIS to view only 1 slice of time period at a time.

### Using Temporal Analysis:

![Temporal analysis 1](https://github.com/Astrojigs/Delhi-air-pollution-reasons/blob/main/Photos/Temporal%20controller%201.png)

#### Temporal Slices:

|August|September|November|
|--|--|--|
|![august slice](https://github.com/Astrojigs/Delhi-air-pollution-reasons/blob/main/Photos/Temporal%20slice%20august.png)|![Sept Slice](https://github.com/Astrojigs/Delhi-air-pollution-reasons/blob/main/Photos/Temporal%20slice%20september.png)|![Nov Slice](https://github.com/Astrojigs/Delhi-air-pollution-reasons/blob/main/Photos/Temporal%20slice%20november.png)|

The visibility of fire increases as we progress through the months from August to December.

The time is perfect to harvest the crops in the month of November to December. Once the process of harvesting is complete, farmers use the same land to grow new crops. This can quickly be done by burning stubbles (what's left of the crop after harvesting). There is an increase in stubble burning during the end of the year. Hence we see more fire incidents taking place during that time.

## So where does the excess smoke and pollution go?

Using wind and terrain data we can manage to find the direction where the pollution can go.

![terrain and wind data in the month of November](https://github.com/Astrojigs/Delhi-air-pollution-reasons/blob/main/Photos/terrain%20with%20wind%20data%20in%20Novemeber.png)

From the above visualization we can see that the direction of wind is going towards the east from west. This takes all the pollution along with it.

In order to provide more evidence for this, I used a simple query to extract pm2.5 particle concentration during that time. it turns out that the QGIS analysis and visualization we did earlier matches with the amount of increase in pm2.5 particles in the air during the end of the year in Delhi.

![pm2.5 pollutant concentration in Delhi](https://github.com/Astrojigs/Delhi-air-pollution-reasons/blob/main/Photos/pm25history%20in%20delhi.png)

The top rows are with high concentration of pm2.4 pollutants. This matches with the pollutants thrown out due to stubble burning in Punjab and Haryana.
