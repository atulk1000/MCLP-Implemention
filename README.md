# Preventive Health Care Facility Location

This repository contains a Jupyter notebook implementation of a preventive health care facility location model based on the framework in Verter and Lapierre (2002), *Location of Preventive Health Care Facilities*.

The notebook uses ZIP Code Tabulation Area (ZCTA) demand and coordinates to choose facility locations that maximize captured demand while enforcing:

- at most one assignment per demand point
- assignment only to open facilities
- a minimum workload threshold for each open facility
- closest-open-facility logic through an iterative cut procedure inspired by Section 4, Procedure I of the paper

## Repository Contents

- `MCLP_Implementation-Final_Master_File_with demand distribution-optimized.ipynb`
- `Philadelphia ZCTAs Sub Service line level_Cardiology_Medical Cardiology.csv`
- `LICENSE`

## Notebook Overview

The notebook:

- reads weighted demand by ZCTA
- computes pairwise distances between demand points and candidate sites
- estimates participation as a linear decay function of distance
- solves a mixed-integer optimization model with OR-Tools
- iteratively adds violated closest-facility constraints
- visualizes selected sites and demand capture

## Model Notes

This implementation is best described as a practical adaptation of the paper rather than a line-by-line reproduction.

Key choices in the notebook:

- `x[i, j]` indicates whether demand point `i` is assigned to facility `j`
- `y[j]` indicates whether facility `j` is opened
- `pfac[i, j]` represents distance-adjusted captured demand from population center `i` to site `j`
- `min_demand` acts as a facility viability threshold

The notebook currently uses the OR-Tools `SCIP` backend by default because it is free and easy to run without a commercial solver license. If you have access to a valid Gurobi license, Gurobi is generally the preferred solver for better performance on larger mixed-integer models.

## Runtime Improvements Included

The current notebook includes several runtime-oriented changes that do not change the model logic:

- binary decision variables are declared with `BoolVar`
- infeasible assignments outside the service radius are omitted from the model
- assignment and workload structures are stored sparsely using feasible `(i, j)` pairs only
- facilities that can never satisfy `min_demand` even under best-case assignment are screened out before solving
- Procedure I uses precomputed site orderings to speed up closest-open-facility checks

These changes are especially helpful when `min_demand` is large.

## Input Data

The notebook expects a CSV with fields including:

- `zcta`
- `weighted demand`
- `internal_pt_lat`
- `internal_pt_lon`

The included sample file is:

- `Philadelphia ZCTAs Sub Service line level_Cardiology_Medical Cardiology.csv`

## Requirements

Install the main Python dependencies before running the notebook:

```bash
pip install ortools haversine numpy pandas matplotlib plotly
```

If you want to test Gurobi separately:

```bash
pip install gurobipy
```

You would also need a valid Gurobi license.

## How To Run

1. Open the notebook in Jupyter Notebook or JupyterLab.
2. Confirm the CSV path points to the local dataset.
3. Run the cells in order from top to bottom.
4. Review the selected facilities, demand capture summaries, and maps.

## Outputs

The notebook produces:

- selected facility locations
- demand-to-facility assignments
- demand capture by open facility
- scatter and geographic plots
- demand capture summaries by ZCTA

## Limitations

- Distances are computed with haversine distance between coordinates, which is an approximation of real travel distance.
- Results depend strongly on `min_demand`, the travel threshold, and the quality of the weighted demand input.
- Procedure I can still take substantial time on larger instances or high minimum-demand thresholds.
- The notebook is intended for research and prototyping, not production deployment.

## Reference

Verter, V., & Lapierre, S. D. (2002). Location of preventive health care facilities. *Annals of Operations Research*, 110, 123-132.
