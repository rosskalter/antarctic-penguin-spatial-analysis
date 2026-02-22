# Antarctic Penguin Spatial Analysis & Habitat Constraints

**Author:** Ross Kalter
**Role:** Geospatial Analyst / Ecologist  
**Tools:** ArcGIS Pro, Spatial Data Management, Cartographic Design  

## Project Overview
This repository contains the data summaries and methodology for a spatial ecology analysis of penguin breeding colonies on the Antarctic Peninsula. The objective was to quantitatively test the hypothesis that active breeding colonies for four species (Adélie, Chinstrap, Gentoo, and Macaroni) are strictly constrained to limited rocky coastal substrates rather than ice-dominated coastlines.

The analysis evaluated over **1.2 million contemporary breeding pairs** and successfully identified a 100% reliance on rocky shores, while also mapping distinct East/West spatial habitat partitioning and localized detection biases.

---

## Data Sources
To ensure contemporary accuracy and reproducibility, all datasets are open-source and were restricted to observations from the year 2015 onwards.

* **Penguin Census Data:** Mapping Application for Penguin Populations and Projected Dynamics ([MAPPPD](https://www.penguinmap.com)). Contains population counts for Adélie, Chinstrap, Gentoo, and Macaroni species.
* **Environmental Geomorphology:** Scientific Committee on Antarctic Research ([SCAR / ADD](https://www.add.scar.org/)). High-resolution vector lines (coastline substrate) and polygons (land cover).
* **Research Facilities:** Council of Managers of National Antarctic Programs ([COMNAP](https://www.comnap.aq/antarctic-facilities-information)).

---

## Geoprocessing Workflow (ArcGIS Pro)
Because this project utilized GUI-based geoprocessing in ArcGIS Pro rather than automated Python/ArcPy scripts, the exact spatial workflow is documented below for reproducibility:

1. **Data Conversion (`XY Table to Point`):** Raw MAPPPD and COMNAP tabular coordinates were projected into spatial vector points. All datasets were standardized to a South Pole Stereographic projection to minimize areal distortion.
2. **Data Isolation (`Digitization` & `Clip`):** A custom bounding box was manually digitized around the northern Antarctic Peninsula. The SCAR continental datasets were then clipped to this geometry to optimize processing times.
3. **Temporal & Demographic Filtering (`Select by Attributes`):** The MAPPPD dataset was queried to exclude historical data and transient non-breeders. Only records from `>= 2015` with `Breeding Pairs > 0` were retained.
4. **Substrate Proximity Analysis (`Spatial Join`):** Colony points were spatially joined to the clipped SCAR coastline lines. To account for field GPS drift and coastline generalization, a `Closest` match option with a maximum search radius of `5 kilometers` was applied.
5. **Statistical Summarization:** The resulting attribute table was exported to generate a pivot table, calculating the sum of breeding pairs grouped by species and coastline type.

---

## Key Findings
* **The Rock Constraint:** 100% of the 1,234,281 analyzed breeding pairs were located on 'Rock' coastline substrates. Zero colonies successfully nested on 'Ice' coastlines, physically validating the biological requirement for pebble-based nest construction.
* **Habitat Partitioning:** Adélie penguins dominate the eastern archipelago with massive "Mega-Colonies" (>50k pairs). Chinstrap and Gentoo penguins partition the western islands into smaller, overlapping "Medium Hubs" (1k-10k pairs).
* **Detection Bias:** The immense spatial clustering of colonies directly correlates with the high density of COMNAP research facilities in the localized grid, suggesting a detection bias driven by logistical accessibility.

---

## Repository Structure
* `/data/` - Contains the final, aggregated `.csv` pivot table detailing population counts by species and substrate. *(Note: Raw SCAR continent-wide shapefiles are omitted due to file size limits; refer to the Data Sources links to download).*
* `/maps/` - High-resolution cartographic exports (PNG/PDF) of the spatial distribution and reference study areas.
