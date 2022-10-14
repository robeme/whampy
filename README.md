# `whampy`
whampy is a python3 script that performs a WHAM calculation on umbrella sampling datasets in order to obtain a potential of mean force.

## Getting Started

### Prerequisites

__Disclaimer:__ _program in development_

* `python3.x` must be installed.
* The following `python` packages must be installed:
  * `numpy`
  * `scipy`
  * `matplotlib`
* These can be installed with `pip`

## `whampy` instructions
The whampy program computes the potential of mean force of an umbrella
sampling simulation using a minimization of a log-likelihood function of
the probability distribution in 1D. 

The execution of the program is as follows:

```shell
python3 wham.py [-h] [-s] [-i INPUT] [-o OUTPUT]
```

where the `INPUT` file is a plain text file with the format as specified
in `wham.in`.  The  trajectory  files sourced from the paths found in 
the input file are assumed to be in the two-column  NAMD .traj format as 
shown examplarily in the `traj/window*.traj` files. 

For more information about the options, the `[-h]` optional flag brings up
the help text.

## Example: Using WHAM in constant potential simulations

I've adapted the original whampy code of enfo14 (https://github.com/enfo14/whampy) (details are found in the subfolder `wham/`) to account for a linear bias potential activated by setting the linear bias flag in the metafile.

* Use the examplary data provided in `traj/window*.traj` stemming from a constant potential simulation of graphite electrodes at various applied electric potentials in contact with 1.5M BMIPF<sub>6</sub> in ACN.
* adapt simulation parameters in wham.in to account for specific needs and your simulation setup. a summary of all available settings is found in /wham/symdata.py
* use flag `#linear = True` in the metafiles to activate a linear bias potential. This interprets the second column in the metafile paths definitions as an applied electrostatic potential.
* run `wham.py -i conp/wham.in -o wham_output` to do the WHAM on the provided data.
* output is at 0.0V. however, one can make use of the fact that generally, the distribution, P(σ), shifts according to applied voltage P(σ|ΔΨ) ∝ P(σ|0V) exp(−σSΔΨ/(kB*T)) (see eq. (3) Merlet, C. et al. The Electric Double Layer Has a Life of Its Own. J. Phys. Chem. C 118, 18291–18298 (2014). This is done in the Jupyter notebook.
* if MC is activated with `#num_mc_runs = 2000` to estimate the confidence, a file containing the bootsrapped PMFs for post processing is stored under `bootstraps.pmf`. Details of this are found in the accompanying Jupyter notebook.
