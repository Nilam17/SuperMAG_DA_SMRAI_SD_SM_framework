# SuperMAG conductance assimilation

 SuperMAG data assimilation for Hall conductance.
 
The same method is applied to two substorms, 10 May 2024 and 10 October 2024. Each event keeps its own solar wind, SuperDARN electric field, and SuperMAG observations. The filter settings are shared.


## Method

The prior Hall conductance comes from the SMRAI echo-state model, driven by the solar wind and dipole tilt, and interpolated onto the SMRAI2.1 grid (magnetic latitude 50–90°, 20 latitude by 72 longitude points).

Conductance updates are written with 1000 spherical radial basis functions on that cap. 
The state is the basis coefficients together with a weight at each center. 
The weight enters the conductance as a multiplicative factor bounded to the range 1/3–3. 
An ensemble transform Kalman filter (1000 members) updates this state every 5 minutes. The observation is SuperMAG dBn. 
The forward model is the Hall current from the local conductance times the poleward electric field, sampled at each station. 
Localization uses a 10° distance scale. A 10-minute memory carries the analysis from one time step to the next, and the forecast covariance is inflated by 1.5. The observation-error standard deviation is 200 nT.

Optional multiplicative perturbations are applied to the solar-wind drivers and to the prior conductance. Both are on by default, with fixed seeds, so the two events stay reproducible.

## Data

**SuperDARN-assimilated electric field.** 

Nilam, B. (2026). *SMRAI with SuperDARN Data Assimilation for Substorm Analysis*. Zenodo. https://doi.org/10.5281/zenodo.18857294

and place the SuperDARN data in `SMRAI2.1/`. Each event needs

`SMRAI2.1/substorm_YYYYMMDD/SMRAI2.1_DAssim/ef_est*.dat`

**Solar wind.** Download the solar-wind file for the event and save it as `sw_data/swdataYYYYMMDD.dat` (or `sw_data/modified/swdataYYYYMMDD.dat`).

**SuperMAG.** Download the 60 s SuperMAG file and save it as `SM_data/YYYYMMDD.vec-supermag.60s.rev-0006.ncdf`.

**Hall-conductance model.** Place these files next to the notebook:

- `SMRAI2/modelw099_300.sav`
- `SMRAI2/meanw.npy`
- `SMRAI2/imgw.npy`

## Run

Open `Supermag_conductance_assimilation_run.ipynb` and run every cell from top to bottom. `event_dates` selects the events. The default list is `2024-05-10` and `2024-10-10`. A date may be written as `20240510`.

The notebook requires Python with `numpy`, `pandas`, `xarray`, `netCDF4`, `scipy`, `geopack`, and `scikit-learn`.

## Output

- `outputs/assimilation_20240510.npz`
- `outputs/assimilation_20241010.npz`

Each file contains the prior and updated Hall conductance, the electric field, SuperMAG dBn with magnetic latitude, magnetic local time, and station IDs, and the forecast and analysis at the stations.
