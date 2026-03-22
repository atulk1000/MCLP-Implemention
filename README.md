# MCLP-Implemention
Predict optimal locations to open healthcare facility in a geographical area 

# Preventive Health Care Facility Location using MCLP

This repository contains a Jupyter notebook implementation of a preventive health care facility location model inspired by Verter and Lapierre (2002), *Location of Preventive Health Care Facilities*.

The notebook solves a maximal covering / facility location style optimization problem to identify candidate health care facility locations that maximize captured demand subject to service accessibility and minimum workload requirements.

## File

- `MCLP_Implementation-Final_Master_File_with demand distribution-with_bug_fixes.ipynb`

## Overview

The model uses ZIP Code Tabulation Area (ZCTA)-level demand and geographic coordinates to:

- estimate demand at each population center
- compute distances between demand locations and candidate facility sites
- assign demand points to facilities
- enforce a minimum workload threshold for any opened facility
- iteratively enforce closest-open-facility assignment logic based on the exact solution approach described in Section 4, Procedure I of Verter and Lapierre (2002)
- visualize selected facility locations and demand capture

## Methodology

This notebook is based on the preventive health care location framework proposed by Verter and Lapierre (2002).

Key modeling elements used here include:

- binary facility-opening decisions
- binary assignment decisions
- distance-based participation / demand capture
- minimum demand threshold for open facilities
- iterative addition of violated closest-facility constraints

The implementation is best described as a practical adaptation of the paper’s Procedure I for experimentation with real geographic demand data.

## Input Data

The notebook expects a CSV input file with fields such as:

- `zcta`
- `weighted demand`
- `internal_pt_lat`
- `internal_pt_lon`

Example input used in the notebook:

- `Philadelphia ZCTAs Sub Service line level_Cardiology_Medical Cardiology.csv`

## Requirements

Install the main dependencies before running the notebook:

## How to Run
Open the notebook in Jupyter Notebook or JupyterLab.
Update the CSV file path if needed.
Run the cells in order.

## Review:
selected facility locations
assignment behavior
demand capture output
geographic visualizations

## Outputs
The notebook produces:
the set of selected facility sites
assignment of demand points to open facilities
estimated demand captured by each selected facility
scatter and geographic plots of chosen locations
summary tables for demand distribution

## Notes and Limitations
This implementation is based on the paper, but may include practical adaptations for experimentation.
Distances are computed using haversine distance between point coordinates.
The quality of the solution depends heavily on the demand model, travel threshold, and minimum demand parameter.
This notebook is intended for research, prototyping, and analytical exploration rather than production deployment.

##Reference
Verter, V., & Lapierre, S. D. (2002). Location of preventive health care facilities. Annals of Operations Research, 110, 123-132.

```bash
pip install ortools haversine numpy pandas matplotlib plotly
