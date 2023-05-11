CAVAanalytics

================
2023-05-01

## Introduction: What is CAVA?

CAVA (Climate and Agriculture Risk Visualization and Assessment) is a
framework and approach to the analysis and visualization of
state-of-the-art climate data. [CAVA is made of a Platform
(GUI)](https://fao-cava.predictia.es/auth), which is freely accessible
and satisfy the needs of most standard users, and a R package
(**CAVAanalytics**) which allows remote access to climate data and
several functions for more advance climate data analysis.

CAVAanalytics can be used locally (installation through GitHub),
remotely (registered users can access the University of Cantabria
JupyterHub with 180 Gb RAM) and through a Docker image.

|                                                                                        ![framework](https://user-images.githubusercontent.com/40058235/234544688-af8c5a1d-588e-4145-813f-feb0ea2db916.png)                                                                                         |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| *CAVA framework. CAVA can be used through a graphic user interface (CAVA Platform) or through the CAVAanalytics R package. The package can be used locally (GitHub installation), remotely (University of Cantabria JupyterHub) or locally but through a Docker image to solve dependencies issue* |

## Current status

**CAVAanalytics is currently in pre-release.** You can install it
locally but only few climate models are available for remote access. It
would work if you have data locally available. Additionally, working
examples will be made available soon in the tutorial folder.

## CAVAanalytics

**CAVAanalytics** is a package that offers a consistent framework to
load, analyse and visualize multi-model ensembles. **CAVAanalytics**
provides an access point for CORDEX-CORE simulations at 25 Km resolution
already interpolated plus the W5E5 and ERA5 datasets. CAVAanalytics can
be seen as a wrapper of several packages, but the main engine for
loading and processing climate models is the [climate4R
framework](https://github.com/SantanderMetGroup/climate4R), applied with
a tidyverse approach.

CAVAanalytics is being developed with climate models in mind, but it can
be used for any model, like impact models (ISIMIP) if data is locally
available.

## Installation

Based on how you want to use CAVAanalytics, there are three options.

### 1) Locally from GitHub

If you have already installed most of the climaterR packages, you can
simply install CAVAanalytics from GitHub:

    library(devtools)
    install_github("Risk-Team/CAVAanalytics")

**If you are also new to climate4R**, install its main packages first.
You can do so by

    install.packages("rJava")
    library(devtools)
    install_github(c("SantanderMetGroup/loadeR.java",
                     "SantanderMetGroup/climate4R.UDG",
                     "SantanderMetGroup/loadeR",
                     "SantanderMetGroup/transformeR",
                     "SantanderMetGroup/climate4R.indices",
                     "SantanderMetGroup/downscaleR"))

it is possible that the installation of loadR.java fails. If so, have a
look at the [loadR main
page](https://github.com/SantanderMetGroup/loadeR) and the [wiki
page](https://github.com/SantanderMetGroup/loadeR/wiki/Installation) to
solve the issue. loadR.java depends on rJava and this installation can
be troublesome.

### 2) JupyterHub

You can request access to the University of Cantabria JupyterHub, where
CAVAanalytics is already installed. This will give you access to
computational resources and you would be able to perform your climate
analysis using a Jupyter Notebook environment. If you would like to
access these resources, you are welcome to contact
<riccardo.soldan@fao.org> or <Hideki.Kanamaru@fao.org>

### 3) Locally through Docker

A docker image will be made available soon, containing all the required
software to immediately start working with CAVAanalytics.

## Using CAVAanalytics

CAVAanalytics makes it easier to work with a large number of climate or
impact model simulations (netCDF files) and perform meaningful analysis.
The idea behind CAVAanalytics is to first load the data (multiple
models) and then work with the output of the load_data with other
**CAVAanalytics** functions.

| ![Framework](https://user-images.githubusercontent.com/40058235/199256415-ed32c42b-e2f8-48e0-b4fe-558de6612038.png) |
|:-------------------------------------------------------------------------------------------------------------------:|
|                                                *CAVAanalytics steps*                                                |

## Examples

### Loading data, processing and visualizing results (more examples in the tutorials folder)

**To load CORDEX-CORE data stored remotely**, set path.to.data to
“CORDEX-CORE” and specify the domain. This will load CORDEX-CORE
simulations. Similarly, when path.to.obs is set to W5E5, you are
accessing the dataset stored remotely.

``` r
library(CAVAanalytics)
  # 1st step
remote.data <- load_data(country = "Sudan", variable="tasmax", years.hist=1995, years.proj=2050:2055,
              path.to.data = "CORDEX-CORE", path.to.obs="W5E5", domain="AFR-22")
  # 2nd step
  projections(remote.data, season=1:12, bias.correction = F) %>% 
  # 3rd step
  plotting(., ensemble=T, plot_titles = "Average tasmax")
```

![](README_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

You can also look at individual models and specify agroclimatic
indicators. For example, number of days with maximum temperature above
45 °C.

``` r
  # 2nd step
  projections(remote.data, season=1:12, uppert=42,  bias.correction = F) %>% 
  # 3rd step
  plotting(., ensemble=F, plot_titles = "N. days Tmax > 45 °C", 
           palette=c("white", "orange", "red", "black"))
```

![](README_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->
