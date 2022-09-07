---
title: First evaluation - Final rush for first milestone, advices from mentors, and keep going!
date: "2022-07-24"
description: The first evaluation is closing in. Tran is quite anxious now, but still, he tries his best to finish this deliverance. Also, he receives heartful reviews and suggestions from his mentors as well. This concludes the first evaluation with something done, something yet to do, and a sheer determination!
---

In this final week, I fully dedicate myself into user-testing cases. As pointed out by Mr. Erwan Pannier, _"Things will definitely change after user feedback so don't waste too much time before that"_. Thus, I must quickly bring up several notebooks for the users - our spectroscopic scientists - to test and provide feedbacks, and from then I can shape my fitting modules closer to the real usages and needs. Firstly, I will talk about several additional updates in this final week of phase 1:

 ### 1. Non-LTE support

 Now the fitting module supports non-LTE input. The module will automatically switch to non-LTE mode when `Tvib` is detected inside the input meterials.

 If there are more than one `Tvib` due to non-diatomic molecules having more than one vibrational temperatures, the input of `Tvib` will be either `tuple` or `list`, and thus needed to be treated differently. For this, the procedure is gonna be:
 1. After `Tvib` is detected as a fitting parameter within `fit`, if it is in type of either `tuple` or `list`, each of the constituting temperature will then be extracted, and assigned a number after it, for example, `Tvib1`, `Tvib2`, and so on.
 2. Then, these separated temperatures will be assigned to a Parameter object and from here, they will be treated as an individual fitting parameter, which will altogether enter the fitting process.
 3. After fitting process, all Parameter objects with `Tvib` in their names will be collected and merged into the original `Tvib` array.

### 2. Format refactor - more flexible ways to input

As I mentioned last week, the new fitting module should also support in-script JSON structure, because as Mr. Erwan Pannier pointed out, people would prefer adjusting everything within a single script, rather than switching between windows just to adjust a separated JSON file. So now the idea is to have 2 ways of input: either through in-script `JSON` structures, or through an external `JSON` file.

Now the users can either use a separated JSON file as input, or directly input a bunch of `dict` parameters.

For example, when using JSON file:

````python
# Get JSON file path (note that the experimental spectrum file MUST BE IN THE SAME FOLDER containing JSON file)
JSON_path = "../test_dir/test_JSON_file.json"

# Conduct the fitting process!
s_best, result, log = fit_spectrum(input_file = JSON_path)
````

And when inputting a bunch of `dict` parameters (pure Python way):

````python
# Load experimental spectrum. You can prepare yours, or fetch one of them in the ground-truth folder like below.
s_experimental = load_spec("./data/LTE/ground-truth/synth-NH3-1-500-2000-cm-1-P10-t1000-v-r-mf0.01-p1-sl1nm.spec")

# Experimental conditions which will be used for spectrum modeling. Basically, these are known ground-truths.
experimental_conditions = {
    "molecule" : "NH3",         # Molecule ID
    "isotope" : "1",            # Isotope ID, can have multiple at once
    "wmin" : 1000,              # Starting wavelength/wavenumber to be cropped out from the original experimental spectrum.
    "wmax" : 1050,              # Ending wavelength/wavenumber for the cropping range.
    "wunit" : "cm-1",           # Accompanying unit of those 2 wavelengths/wavenumbers above.
    "mole_fraction" : 0.01,     # Species mole fraction, from 0 to 1.
    "pressure" : 10,            # Partial pressure of gas, in "bar" unit.
    "path_length" : 1,          # Experimental path length, in "cm" unit.
    "slit" : "1 nm",            # Experimental slit, must be a blank space separating slit amount and unit.
    "offset" : "-0.2 nm"        # Experimental offset, must be a blank space separating offset amount and unit.
}

# List of parameters to be fitted.
fit_parameters = {
    "Tgas" : 700,               # Fit parameter, accompanied by its initial value.
}

# List of bounding ranges applied for those fit parameters above.
bounding_ranges = {
    "Tgas" : [600, 2000],       # Bounding ranges for each fit parameter stated above. You can skip this step, but not recommended.
}

# Fitting pipeline setups.
fit_properties = {
    "method" : "lbfgsb",        # Preferred fitting method from the 17 confirmed methods of LMFIT stated in week 4 blog. By default, "leastsq".
    "fit_var" : "radiance",     # Spectral quantity to be extracted for fitting process, such as "radiance", "absorbance", etc.
    "normalize" : False,        # Either applying normalization on both spectra or not.
    "max_loop" : 150,           # Max number of loops allowed. By default, 100.
    "tol" : 1e-10               # Fitting tolerance, only applicable for "lbfgsb" method.
}

# Conduct the fitting process!
s_best, result, log = fit_spectrum(
    s_exp = s_experimental,
    fit_params = fit_parameters,
    bounds = bounding_ranges,
    model = experimental_conditions,
    pipeline = fit_properties
)
````

As we can see, this will be much more flexible for the users to input the ground-truth data and parameters.

### 3. Sample notebooks and test files

To support the user-testing process, 4 sample Jupyter notebooks and 4 corresponding Python test files have been added. They are:
- [`sample_LTE_Tgas.ipynb`](sample_LTE_Tgas.ipynb) : LTE fitting case, with `Tgas` as fit parameter, multiple-dict input.
- [`sample_LTE_Tgas-molfrac.ipynb`](sample_LTE_Tgas-molfrac.ipynb) : LTE fitting case, with `Tgas` and `mole_fraction` as fit parameters, multiple-dict input.
- [`sample_LTE-with-JSON_Tgas.ipynb`](./sample_LTE-with-JSON_Tgas.ipynb) : LTE fitting case, with `Tgas` as fit parameter, JSON file input.
- [`sample_nonLTE_1Tvib-Trot.ipynb`](./sample_nonLTE_1Tvib-Trot.ipynb) : non-LTE fitting case, with 1 `Tvib` and 1 `Trot` as fit parameters, multiple-dict input.

Also, as the first evaluation is drawing near, and the fact that my fitting module for 1st phase is fundamentally finished (there might be some bugs, but now it's just time for bug reporting, debugging and keep it that way), I believe now it's a good time for me to reflect my progress during this first phase, and to plan accordingly for the next phase. Thus, I decide to ask my mentors to discuss about the current progress, and future plans as well. Also, I hope to receive several reviews about my work attitude, current impression of phase 1, and the current state of evaluation and room for improvements, too. Turned out that the comments are quite good, better than I expected. The only thing that I need to improve is, welp, don't asking _too much_ feedbacks, but instead just focus on the content and quality of the module development.

I would like to conclude this final blog of phase 1 with words from Mr. Erwan Pannier:

> In other words : this is not a school-project. There is no right/wrong/expectations that you should.
> Just work, deliver, try things, fail, change direction, try again, succeed. But focus on the content, and focus on delivering a great product.
> In your case, a great fitting routine. When you will be happy with what you've done, people will follow.