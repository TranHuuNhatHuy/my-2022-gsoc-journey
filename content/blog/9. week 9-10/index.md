---
title: Week 9, 10 and 11 - The challenge, the calamity, the hope, and the salvation.
date: "2022-08-28"
description: What could be worse than a very challenging coding task? It is a very challenging coding task while your computer is down.
---

### 1. The challenge

After successfully dealing with Mr. Minou's case, I got a message from another RADIS spectroscopic scientist - Mr. Corentin Grimaldi, a Ph.D. candidate of CentraleSupélec, Paris. He had several experimental spectra containing CO and CO2 at different temperatures and possibly in non-equillibrium. There were 3 spectra in total, and he also provided 3 Python scripts he used to fit them:

- `Fit_init_V2.py` : the script for fitting initialization, which conducts all the basic functions such as normalization, crop, slit convolution & dispersion, residual calculation, etc.
- `Optimize_find_min_2D_V2.py` : the program that looks for optimum parameters such as `Trot`, `Tvib`, `molfrac`, etc.
- `fit_CO_1T_CO2_2T_cv_V2.py` :  the master program that conducts main fitting job.

So there is one thing that I need to mention first: these spectra are quite challenging to fit. Not hard, but challenging. Both CO2 and CO overlap in this spectral region, and a lot of parameters are unknown: `Trot`, `Tvib`, `x_CO` for CO and `Trot`, `Tvib1`, `Tvib2`, `Tvib3`, `x_CO2` for CO2. Furthermore, unfortunately some cold CO2 and H2O absorb the incomming radiance and the mole fraction is also unknown, too. So basically, there are a lot of fit parameters we need to find, and they don't really follow the normal spectrum calculation that RADIS offers. 

Although Mr. Corentin provided a fitting pipeline for these cases, I still kinda doubt whether should I implement his very specific and exotic approach to my unified modules. After experiencing a lot of fitting scripts from various users in the community, I feel like although I can implement a common interface to support most of fitting cases, but definitely not all of them, since each experimental spectrum is suitable for a unique workflow. Mr. Corentin's case and approach is just somewhat way too unique and completely different from mine. So basically, his approach is quite difficult to be generalized and implemented into a module aimed to serve general fitting cases.

### 2. The calamity

After spending the whole week 9 finding a solution for Mr. Corentin's cases, at the beginning of week 10, all of sudden my MSI laptop - the 6-year buddy always accompanies me to all the coding contests and evreything - stopped working. The next day, the iPhone 11 I bought this February had its screen flickered intensively and thus unable to use. Within less than 48 hours, I, one of GSoC contributor, was cut off from the modern life! All the following days were desperate efforts trying to get my stuffs fixed but nothing worked, trying to communicate with the mentors, trying not to miss any important emails and news. Amidst the challenging second phase of GSoC, I was rendered useless for one week straight! I shudder recalling it, those dark and desperate days sitting in Japanese internet cafes to access the Internet, while extremely anxious about a future of failing GSoC.

### 3. The hope

After one week of despair, my new Macbook Air M1 finally arrived!

[My new buddy!](./new_Mac.jpg)

Finally days and nights in extreme anxious of being unable to do anything while seeing my friends committing and pushing onto GitHub, have finally gone! Now I have my new Mac (with an exorbitant cost as Japanese Yen is dropping like my mental condition recently), and within the remaining days until the final evaluation, I will rush my best with all I have to deliver my module!

### 4. The salvation

After a long time of struggling, I finally received new experimental spectra from Mr. Corentin. He finally realized that those old cases were just too complicated, and now we have 4 simpler tests case with only CO, in the spectral range of 2000 nm to 2600 nm, non-equillibrium, not really absorbed:

1. [0_100cm%20Down%20Sampled%20-%2010cm_10pctCO2_1-wc-gw450-gr300-sl1500-acc5000-.spec](./0_100cm%20Down%20Sampled%20-%2010cm_10pctCO2_1-wc-gw450-gr300-sl1500-acc5000-.spec)

2. [0_100cm%20Down%20Sampled%20-%2020cm_10pctCO2_1-wc-gw450-gr300-sl1500-acc5000-.spec](0_100cm%20Down%20Sampled%20-%2020cm_10pctCO2_1-wc-gw450-gr300-sl1500-acc5000-.spec)

3. [0_200cm%20Down%20Sampled%20-%2035cm_10pctCO2_1-wc-gw450-gr300-sl1500-acc5000-.spec](./0_200cm%20Down%20Sampled%20-%2035cm_10pctCO2_1-wc-gw450-gr300-sl1500-acc5000-.spec)

4. [0_300cm%20Down%20Sampled%20-%2010cm_10pctCO2_1-wc-gw450-gr300-sl1500-acc5000-.spec](./0_300cm%20Down%20Sampled%20-%2010cm_10pctCO2_1-wc-gw450-gr300-sl1500-acc5000-.spec)

Also, in these cases Mr. Corentin introduced a complex slit settings like this:

````python
def slit_dispersion(w):
    phi = -6.33
    f = 750
    gr = 300
    m = 1
    phi *= - 2*np.pi/360
    d = 1e-3/gr
    disp = w/(2*f)*(-np.tan(phi)+np.sqrt((2*d/m/(w*1e-9)*np.cos(phi))**2-1))
    return disp  # nm/mm

def apply_my_slit(spectrum, inplace=False):
    slit = 1500  # µm
    pitch = 20   # µm
    top_slit_um = slit - pitch   # µm
    base_slit_um = slit + pitch  # µm
    center_slit = 5090
    dispersion = slit_dispersion(center_slit)
    top_slit_nm = top_slit_um*1e-3*dispersion
    base_slit_nm = base_slit_um*1e-3*dispersion*1.33
    return spectrum.apply_slit((top_slit_nm, base_slit_nm), center_wavespace=center_slit, unit='nm', shape='trapezoidal', slit_dispersion=slit_dispersion, inplace=inplace)
````

Initially, my fitting module only allows `slit` to be enter with the format of a slit value accompanied by slit unit. For example:

````python
experimental_conditions = {
    ....
    "slit" : "-0.2 nm",
    ....
}
````

To support Mr. Corentin's input, from now I implement input of complex slit settings in compliance with [apply_slit()](https://radis.readthedocs.io/en/latest/source/radis.spectrum.spectrum.html#radis.spectrum.spectrum.Spectrum.apply_slit) function of RADIS. Advanced settings such like this can be inputted:

````python
experimental_conditions = {
    ....
    "slit" : {
        "slit_function" : (top_slit_nm, base_slit_nm),
        "unit" : "nm",
        "shape" : 'trapezoidal',
        "center_wavespace" : center_slit,
        "slit_dispersion" : slit_dispersion,
        "inplace" : False,
    }
    ....
}
````

And so, I have acquired very good results:

#### Spectrum 1:

- Tvib:             5975.28759 (init = 6000)
- Trot:             5751.19260 (init = 4000)
- mole_fraction:    0.05501671 (init = 0.1)

[Fit result of spectrum 1](./s1.png)

#### Spectrum 2:
- Tvib:             4547.43903 (init = 6000)
- Trot:             4073.50694 (init = 4000)
- mole_fraction:    0.05939918 (init = 0.1)

[Fit result of spectrum 2](./s2.png)

#### Spectrum 3:

- Tvib:             2811.98218 (init = 6000)
- Trot:             2915.36318 (init = 4000)
- mole_fraction:    0.07739941 (init = 0.05)

[Fit result of spectrum 3](./s3.png)

#### Spectrum 4:

- Tvib:             4721.28892 (init = 6000)
- Trot:             4728.52960 (init = 4000)
- mole_fraction:    0.07008355 (init = 0.05)

[Fit result of spectrum 4](./s4.png)