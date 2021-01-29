# pygrutils: an assortment of graphics utilities

![version](https://img.shields.io/badge/version-v0.1.1-blue)
[![license: MIT](https://img.shields.io/badge/license-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Language grade: Python](https://img.shields.io/lgtm/grade/python/g/ttesileanu/pygrutils.svg)](https://lgtm.com/projects/g/ttesileanu/pygrutils/context:python)

This is an assortment of utilities for plotting in Python using [`Matplotlib`](https://matplotlib.org/) and [`seaborn`](https://seaborn.pydata.org/generated/seaborn.regplot.html?highlight=regplot#seaborn.regplot). Below are some highlights.

## Features

### A more useful `regplot`

The [`seaborn`](https://seaborn.pydata.org/) function [`regplot`](https://seaborn.pydata.org/generated/seaborn.regplot.html?highlight=regplot#seaborn.regplot) allows overlaying a fit line and confidence interval on a scatter plot. Unfortunately, it does not either a) provide a mechanism to access the fitting results, or b) allow plotting a user-provided fit line. This package provides a reimplementation [*] of `sns.regplot` that returns the `RegressionResults` structure from a [`statsmodels`](https://www.statsmodels.org/stable/index.html) linear fit. The implementation also provides functions `scatter`, `fitplot`, and `polyfit`, that are used by `regplot` to generate the scatter part of the plot, the fit line and confidence interval, and to obtain the fitting results, respectively.

[*] Some features of `sns.regplot` are not currently implemented. Others may behave slightly differently.

The `regplot` function provided here should function as a drop-in replacement for `sns.regplot` in most cases. The main disadvantage is that the styling options are slightly different, and so the results might not always match perfectly. In some cases this is by design (because I didn't like `seaborn`'s choices), but in other cases it's because I haven't yet implemented all the hacks that `seaborn` employs to yield good-looking plots.

There are some things that this `regplot` function does better than `sns.regplot`:

* **consistency:** all the fits are calculated using `statsmodels`, even the polynomial ones.
* **speed:** confidence intervals are calculated directly using `statsmodels`, removing the need for bootstrapping.
* **flexibility:** polynomial fits in `log(x)` work are not allowed in `sns.regplot` but work here.
* **configurability:**

  * the number of points used for the fit line and confidence interval is configurable;
  * separate keyword options for confidence intervals are supported.

## Installation

After cloning the repository or downloading and decompressing, run the following command in the folder containing `setup.py`:

    pip install .

## Usage

The basic usage is identical to `seaborn`, *e.g.*:

    import matplotlib.pyplot as plt
    import pygrutils as gr
    import numpy as np

    # generate some data
    rng = np.random.default_rng(0)
    x = np.linspace(0, 1, 100)
    y = 3.0 * x - 0.15 + rng.normal(size=len(x))

    # plot it
    fig, ax = plt.subplots()
    res = gr.regplot(x, y, order=2, ax=ax)

will make a scatter plot of `y` *vs.* `x`, fitting a second-order polynomial through the data:

![regplot example](img/regplot_example.png)

The `statsmodels` results structure contains a wealth of information:

    res.summary()

![regplot example stats](img/regplot_example_stats.png)

More examples can be found in the notebooks in the `test` folder.
