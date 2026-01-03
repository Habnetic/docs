# Datasets List

This is a human-readable index of datasets used or planned for Habnetic benchmark sites.
Operational details live in the Habnetic/data repository.

## Sites
- RTM (Rotterdam)
- SFO (San Francisco)
- CON (Concordia Station, Antarctica) – future
- LUN (Moon, synthetic habitat)
- MAR (Mars, synthetic habitat)

## RTM (planned / in progress)
- ERA5 reanalysis (precipitation, temperature) – climatic forcing
- KNMI climate and wind observations – national reference
- Rotterdam Open Data waterways and water-adjacent constraints – regulatory flood exposure proxy
- Rijkswaterstaat rivers, canals, and coastal defense layers – infrastructural context
- OpenStreetMap building footprints – exposure
- CBS Netherlands population and housing statistics – socio-demographic covariates

## SFO (planned / in progress)
- USGS ShakeMap / PAGER – seismic intensity and exposure proxies
- San Francisco Open Data building footprints – exposure / typology proxy
- NOAA / FEMA coastal surge zones – coastal hazard extent
- Census TIGER / ACS – population and socioeconomic covariates

## CON (planned / future)
- Station layout and infrastructure schematics – exposure proxy
- ERA5 / polar climate reanalysis – environmental forcing
- Procedural outage and failure scenarios – isolation and system stress testing

## LUN (synthetic / planned)
- Procedural hazard fields (radiation, thermal cycling, micrometeoroids)
- Synthetic habitat layouts and component hierarchies
- Optional lunar DEM tiles – terrain and siting context

## MAR (synthetic / planned)
- Procedural hazard fields (radiation, dust storms, thermal extremes)
- Synthetic settlement layouts and recovery timelines
- Optional Mars DEM tiles – terrain and environmental context

## Notes
Each dataset should have:
- provenance (source + retrieval date)
- license
- raw path and processing path
- minimal validation checks (CRS, bbox, missingness)
