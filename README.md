
<!-- README.md is generated from README.Rmd. Please edit that file -->
<!-- devtools::rmarkdown::render("README.Rmd") -->
<!-- Rscript -e "library(knitr); knit('README.Rmd')" -->
Mini `supersmartR` Workshop <img src="https://raw.githubusercontent.com/AntonelliLab/supersmartR/master/logo.png" height="300" align="right"/>
==============================================================================================================================================

> Originally put together for a mini-workshop on 8 Nov. 2019 in Gothenburg, Sweden.

[`supersmartR`](https://github.com/AntonelliLab/supersmartR) is a series of R packages that form a phylogenetic pipeline.

<img src="https://raw.githubusercontent.com/AntonelliLab/supersmartR/master/supersmart%20vs%20supersmartr.png" height="450" align="middle"/>
--------------------------------------------------------------------------------------------------------------------------------------------

This workshop we will introduce the packages and provide code to run a simple pipeline to create a phylogenetic tree.

Workshop duration ~1 hr.

------------------------------------------------------------------------

Prerequisites
=============

-   Software
    -   [R](https://cran.r-project.org/) (&gt; 3.5)
    -   [RStudio](https://www.rstudio.com/)
    -   [Desktop Docker](https://docs.docker.com/install/) (Linux containers)
-   Basic knowledge
    -   R
    -   Phylogenetics

(Windows users may struggle installing Docker Desktop, in which case Docker Toolbox will also work.)

### R packages

Install dependant packages.

``` r
# remotes allows installation via GitHub
if (!'remotes' %in% installed.packages()) {
  install.packages('remotes')
}

library(remotes)
# install latest outsider
install_github("antonellilab/outsider.base")
install_github("antonellilab/outsider")
# install latest restez
install_github("hannesmuehleisen/MonetDBLite-R")
install_github("ropensci/restez")
# install latest phylotaR
install_github("ropensci/phylotaR")
# install latest gaius
install_github("antonellilab/gaius")
```

If you're computer is set-up correctly with the R packages and Docker, you should be able to run the below script without errors.

``` r
## Code
library(outsider)
#> ----------------
#> outsider v 0.1.0
#> ----------------
#> - Security notice: be sure of which modules you install
repo <- "dombennett/om..hello.world"
module_install(repo = repo, force = TRUE)
hello_world <- module_import(fname = "hello_world", repo = repo)
hello_world()

## Output
#> Hello world!
#> ------------
#> DISTRIB_ID=Ubuntu
#> DISTRIB_RELEASE=18.04
#> DISTRIB_CODENAME=bionic
#> DISTRIB_DESCRIPTION="Ubuntu 18.04.1 LTS"
```

------------------------------------------------------------------------

Tutorials
=========

Packages
--------

### `phylotaR`

### `restez`

### `outsider`

### `gaius`

Pipeline
--------

A complete pipeline for constructing a phylogenetic tree of all Caviomorpha species is contained in `pipeline/`. The pipeline uses the above packages and their functions. We can run each script of the pipeline using `source()`. (To save time we will skip the `restez` and `phylotaR` steps and download a completed folder of the first part of the results.)

``` r
## Code

# minor outsider set-up
if (file.exists('gc_setup.R')) {
  source(file = 'gc_setup.R', local = FALSE, echo = FALSE, print.eval = FALSE)
}

# save time by skipping the first two stages by using the ready
#  1_phylotaR/ in data/
zipfile <- file.path('data', '1_phylotaR.zip')
if (file.exists(zipfile)) {
  utils::unzip(zipfile = zipfile, exdir = 'pipeline', overwrite = TRUE)
}

# run each script in the pipeline
stage_scripts <- c('2_clusters.R', '3_align.R', '4_supermatrix.R',
                   '5_phylogeny.R', '6_supertree.R', '7_view.R')
stage_scripts <- c('2_clusters.R',  '4_supermatrix.R',
                    '6_supertree.R', '7_view.R')
start_time <- Sys.time()
cat('Running pipeline ....\n', sep = '')
for (stage_script in stage_scripts) {
  cat('... ', crayon::green(stage_script), '\n', sep = '')
  scriptenv <- new.env()
  source(file.path('pipeline', stage_script), local = scriptenv,
         echo = FALSE, print.eval = FALSE)
}
end_time <- Sys.time()
duration <- difftime(end_time, start_time, units = 'mins')
cat('Duration: ', crayon::red(round(x = duration, digits = 3)), ' minutes.\n')

# clear-up
outsider::ssh_teardown()

## Output
#> Running pipeline ....
#> ... 2_clusters.R
#> ... 4_supermatrix.R
#> ... 6_supertree.R
#> ... 7_view.R
#> Duration:  0.197  minutes.
```

![supertree](supertree.png)

------------------------------------------------------------------------

**Learn more: [`supersmartR` GitHub Page](https://github.com/AntonelliLab/supersmartR)**
