# Welcome to RECON's drat repository

## Guide for users

### Why is it useful?

As many other packages, RECON packages have essentially two main versions:

- a *stable* version on CRAN, which is updated periodically

- a *devel* version on github, which is stable too but incorporates the latest
  additions, features and bug fixes; by convention, the corresponding branches
  are tagged as `@release` for RECON packages

Usually, using the *devel* versions of RECON packages requires the user to
manually install the right version of the package. Moreover, this will usually
default to the `@master` branch, which may not be stable yet. `drat` is a
reliable alternative to install the 'right' *devel* versions automatically when
installing R packages.


### How to use it

This `drat` repository can be used for installing devel versions of RECON
packages automatically pointing to the relevant branch, typically
`@release`. You will first need to install `drat`, which you can do within R
using:


```r
install.packages("drat")
```

Then to install development version of our packages, e.g. the `nomad` package
here, you can type:


```r
drat::addRepo("reconhub")
install.packages("nomad")
```

As `nomad` is not on CRAN, the fact that this command line works will indicate
that the `drat` repository has been used whilst installing the package.



### Using devel versions by default

If you want to make sure you are always using the latest features of RECON
packages, you can register this `drat` repository by default when starting a new
R session by adding the following lines to your `.Rprofile` file:


```r
if (requireNamespace("drat", quietly = TRUE)) {
  drat::addRepo("reconhub")
  message("registered drat repo; devel versions of RECON packages will be used")
} else {
  message("drat is not installed; CRAN versions of RECON packages will be used")
}
```



## Guide for contributors

### Dependencies

We use [`drat.builder`](https://github.com/richfitz/drat.builder) to build the
`drat` infrastructure. This package can be installed using `devtools` by typing:


```r
devtools::install_github("richfitz/drat.builder")
```


### Adding a package

The prefered mechanism for contributions is via *pull requests* against the
`master` branch of this repository. 


To add a package to the `drat` archive, edit the file `packages.txt`, using the
syntax `[github user/organisation]/[package name]@[branch/commit]`, where the
branch/commit indication is optional; if missing, the `master` branch will be
used.

Once you saved the new version of `packages.txt`, open a terminal and execute
the script `drat.builder` by typing: `./drat.builder`. Alternatively, open an R
session and type:


```r
library(methods)
drat.builder:::main()
```

Note that dependencies can be a headache when building the `drat`
infrastructure. This can be particularly problematic when compiling
vignettes. To disable the compilation of vignettes, you can append
`{"vignettes": false}` to the package name (on the same line) in the
`packages.txt` file; for instance:

```
## RECON packages

reconhub/outbreaks@release
reconhub/incidence@release
reconhub/epicontacts@release {"vignettes": false}
reconhub/epitrix@release
reconhub/earlyR@release
reconhub/projections@release
reconhub/dibbler@release
reconhub/vimes@release
reconhub/branchr@release
reconhub/nomad@release
reconhub/recontools@release
reconhub/shinyHelpers@release
reconhub/recon.ui@release
reconhub/incidence.ui@release
reconhub/epicontacts.ui@release

```

indicates that vignettes will not be compiled for `epicontacts`.

