---
title: Fifth week - Major updates on literally everything, and Non-LTE spectra
date: "2022-07-17"
description: With his success in LTE benchmarking as the fitting module run smoothly and pumped out beautiful fit results, Tran now happily moves to the non-LTE benchmarking. He also receives various helpful reviews and suggestions from his mentors, and so the fitting module keeps being developed...
---

In this week, I bring upon a major update that includes literally everything: from the current JSON structure, to the fitting module and its accompanied models, as well as to add the final part of LTE spectra benchmarking result, hence completing my work on LTE spectra, and continue my work on non-LTE spectra.

 ### 1. JSON structure

 As you know, in previous structure, the properties relevant to fitting pipeline (`cutoff`, `slit`, `normalize`, etc.) are hard-coded into the module. This new JSON structure will grant users more flexibility in defining their fitting pipeline. For example, this is a new JSON structure:

 ````json
 {
  "fileName": "CO2_measured_spectrum_4-5um.spec",
  "molecule": "CO2",
  "isotope": "1,2",
  "wmin": 4167,
  "wmax": 4180,
  "wunit": "nm",
  "pressure": 5e-3,
  "mole_fraction": 1,
  "path_length": 10,
  "cutoff": 1e-25,
  "wstep": 0.001,
  "fit": {
    "Tgas": [1000, 2000]
  },
  "pipeline": {
    "method": "leastsq",
    "fit_var": "radiance",
    "normalize": true,
    "max_loop": 100
  },
  "modeling": {
    "offset": "-0.2 nm",
    "slit": "1.4 nm"
  }
}
 ````

As we can see, there are 2 new key sections in the structure:

- `pipeline` : including features for refinement of experimental spectrum after it is collected, such as:
    - `method` : (optional) preferred fitting method from the 17 confirmed methods of LMFIT stated in week 4 blog. `leastsq` by default.
    - `fit_var` : (optional) spectral quantity to be extracted for fitting process, such as `radiance`, `absorbance`, etc. If not stated, it will take the first spectral quantity contained inside the experimental spectrum, assuming it only has one.
    - `normalize` : (required) if `True`, normalize both experimental and model spectra for each fitting loop, else no.
    - `max_loop` : (optional) maximum number rof fitting loops allowed. 100 by default.
    - `tol` : (optional) max fitting tolerance, only applicable if `method == "lbfgsb"` because this is an exclusive feature to L-BFSG-B algorithm.
- `modeling` : including features for refinement of model spectra during fitting loops, such as:
    - `offset` : (optional) for applying offset. There must be a blank space separating offset amount and unit.
    - `slit` : (optional) for applying slit. There must be a blank space separating slit amount and unit.

Also, this update brings forth new available syntaxes for the fit parameters. For each fit parameter stated in `fit`, for example, `Tgas`:

- If `Tgas` is numeric (for example, `Tgas = 1350`) : take this as initial value, with a default bounding width of 500 (so in this case bounding range is [850 , 1850].
- If `Tgas` is either `list` or `tuple` (for example, `Tgas = [1200, 1600]`) : take this as initial bounding range, and the initial value will be the range's midpoint (so in this case initial value is 1400).
- (not yet implemented) If Tgas is a string (for example, `Tgas = "2*Trot`) : take this as dynamic expression and use DynVar of Fitroom. I will try to implement this idea as soon as possible.

Other properties will be parsed directly to [SpectrumFactory object](https://radis.readthedocs.io/en/latest/source/radis.lbl.factory.html#radis.lbl.factory.SpectrumFactory) which means the users will have to refer to the class syntax (but a pre-made JSON form will be provided later with full instructions from me, so don't worry, it's quite user-friendly).

These major changes in JSON structure (and how it is parsed in fitting module) will allow much greater freedom for users to adjust fitting properties.

### 2. Fitting module

Instead of calling `calc_spectrum()` in every fitting loop, now the new module will generate a `SpectrumFactory` object before commencing the fitting loops. This will save a significant time in reloading the database, as well as keeping the source code organized thoroughly.

The new fitting module also supports new JSON structure, and makes sure to parse its information correctly, aiming to limit user-errors. So now the idea is to have 2 ways of input: either through in-script `JSON` structures, or through an external `JSON` file. This will be expressed in the next week.

### 3. Non-LTE benchmarking

According to the benchmarking result, it seems that `leastsq` and `lbfgsb` continue to dominate the overall performance ranking. Compared to LTE, non-LTE cases are usually longer and require more iterations to finish (of course, they have more fitting parameters). After all, I have decided to set the `leastsq` as the default universal fitting method for all cases, in case users don't state the method explicitly in JSON file. Later on, in the fitting tutorial, I will add some suggestions about using `lbfgsb` and trying to switch the `normalize` in case their fitting work is not quite good.