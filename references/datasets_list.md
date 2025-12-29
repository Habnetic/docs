# Datasets List

This is a human-readable index of datasets used or planned for Habnetic benchmark sites.
Operational details live in the Habnetic/data repository.

## Sites
- DSS (Donostia–San Sebastián)
- SFO (San Francisco)
- SYN (Synthetic / Lunar analog)
- CON (Concordia Station) – future

## DSS (planned / in progress)
- ERA5 reanalysis (precip, temp) – forcing
- Basque Government flood hazard zones – vector hazard extent
- Copernicus EMS flood maps – raster event products
- OpenStreetMap building footprints – exposure
- EU/INE population grid – exposure proxy / socio

## SFO (planned / in progress)
- USGS ShakeMap/PAGER – seismic intensity + exposure proxies
- SF Open Data building footprints – exposure / typology proxy
- NOAA/FEMA surge zones – coastal hazard extent
- Census TIGER/ACS – population + socioeconomic covariates

## SYN (planned / in progress)
- Procedural hazard fields (noise-based) – controlled experiments
- Optional lunar DEM tiles – terrain seed

## Notes
Each dataset should have:
- provenance (source + retrieval date)
- license
- raw path and processing path
- minimal validation checks (CRS, bbox, missingness)

