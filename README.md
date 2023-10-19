
<!-- README.md is generated from README.Rmd. Please edit that file -->

# rjdemetra3

**rjdemetra3** offers several functions to interact with JDemetra+ v3.0
workspaces. Seasonal adjustment with X-12ARIMA can be done with the
package [**rjd3x13**](https://github.com/rjdemetra/rjd3x13) and with
TRAMO-SEATS with the package
[**rjd3tramoseats**](https://github.com/rjdemetra/rjd3tramoseats).

## Installation

**rjdemetra3** relies on the
[**rJava**](https://CRAN.R-project.org/package=rJava) package and Java
SE 17 or later version is required.

To get the current stable version (from the latest release):

``` r
# install.packages
remotes::install_github("rjdemetra/rjd3toolkit@*release")
remotes::install_github("rjdemetra/rjd3tramoseats@*release")
remotes::install_github("rjdemetra/rjd3x13@*release")
remotes::install_github("rjdemetra/rjd3providers@*release")
remotes::install_github("rjdemetra/rjdemetra3@*release")
```

To get the current development version from GitHub:

``` r
# install.packages("remotes")
remotes::install_github("rjdemetra/rjdemetra3")
```

## Usage

**rjdemetra3** relies on the
[**rJava**](https://CRAN.R-project.org/package=rJava) package and Java
SE 17 or later version is required.

``` r
library("rjdemetra3")

dir <- tempdir()

y <- rjd3toolkit::ABS$X0.2.09.10.M
jws <- .jws_new()
jsap1 <- .jws_sap_new(jws, "sa1")
add_sa_item(jsap1, name = "x13", x = rjd3x13::x13(y))
add_sa_item(jsap1, name = "tramo", x = rjd3tramoseats::tramoseats(y))
save_workspace(jws, file.path(dir, "wk.xml"))

jws <- .jws_load(file = file.path(dir, "wk.xml"))
.jws_compute(jws) # to compute the models
jsap1 <- .jws_sap(jws, idx = 1) # first SAProcessing
jsa1 <- .jsap_sa(jsap1, idx = 1) # first SAItem
.jsa_name(jsa1)
#> [1] "x13"
mod1 <- .jsa_read(jsa1)
```
