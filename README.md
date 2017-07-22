
<!-- README.md is generated from README.Rmd. Please edit that file -->
rrtools: Tools for Writing Reproducible Research in R
=====================================================

[![Travis-CI Build Status](https://travis-ci.org/benmarwick/rrtools.svg?branch=master)](https://travis-ci.org/benmarwick/rrtools)

The goal of rrtools is to provide instructions, templates and functions for making a basic compendium suitable for doing reproducible research with R. This package documents the key steps and provides convenient functions for quickly creating a new research compendium. The approach is based generally on Kitzes et al. (2017), and more specifically on Marwick (2017) and Wickham's (2017) work using the R package structure as the basis for a research compendium

rrtools gives you a template for doing scholarly writing in literate programming environment using R Markdown and bookdown. It also provides isolation of your computational environment using Docker, package versioning using MRAN, and continuous integration using Travis. It makes a convenient starting point for writing a journal article, report or thesis.

This project was developed during the 2017 [ISAA Kiel](https://isaakiel.github.io/) Summer School on Reproducible Research in Landscape Archaeology at the Freie Universität Berlin (17-21 July). Special thanks to [Sophie C. Schmidt](https://github.com/SCSchmidt) for help. The convenience functions in this package are derived from similar functions in Hadley Wickham's `devtools` package.

Installation
------------

You can install rrtools from github with:

``` r
# install.packages("devtools")
devtools::install_github("benmarwick/rrtools")
```

How to use
----------

To create a reproducible research compendium using the rrtools approach, follow these steps (in [RStudio](https://www.rstudio.com/products/rstudio/#Desktop), which we recommend):

#### 1. `devtools::create("pkgname")`

-   this creates a basic R package with the name pkgname
-   we need to look into the new pkgname directory and then,
-   we need to double-click the `pkgname.Rproj` just created to open the new package project
-   edit the DESCRIPTION to give correct metadata
-   then we continuinously update `Imports:` with names of pkgs used in Rmd, as we write the Rmd, this can be done with, for example, `devtools::use_package("dplyr", "imports")`

#### 2. `devtools::use_mit_license(copyright_holder = "My Name")`

-   this gives MIT licence in DESCRIPTION, adds LICENSE file with our name in it

#### 3. `devtools::use_github(".", auth_token = "xxxx")`

-   This will initialize a local git repository, and connect to github.com and create a remote repository there.
-   we need to get a token from <https://github.com/settings/tokens>, and replace "xxxx" with your token
-   we found that it makes our GitHub repo private by default, so we need to go to GitHub to make it public if we want to use Travis
-   we found that it gives an error in RStudio and doesn't fully enable git in RStudio, so:
-   in the shell, we need to `git remote set-url origin https://github.com/username/pkgname.git` (it does seem to work again in RStudio after completing one commit-push cycle from the shell and restarting RStudio)
-   then we can commit, push, etc. as usual, from the shell or RStudio

#### 4. `devtools::use_readme_rmd(); devtools::use_code_of_conduct()`

-   this makes readme.Rmd, ready for to add markdown code to show travis badge
-   we need to paste in test from CoC from fn output in console, ready for public contributions
-   we need to render this after each change to refresh the readme.md, which is the doc that GitHub shows on the home page of our repository.

#### 5. `rrtools::use_travis()`

-   this creates a minimal .travis.yml for us
-   we need to go to the <https://travis-ci.org/> to connect to our repo
-   we need to add environment variables to enable push of the Docker container to the Docker Hub
-   we need to make an account at <https://hub.docker.com/> to host our Docker container

#### 6. `rrtools::use_analysis()`

-   this creates a top-level `analysis/` directory with the following contents:

<!-- -->

    analysis/
    |
    ├── paper/
    │   ├── paper.Rmd         # this is the main document to edit, could be multiple
    │   ├── references.bib    # this contains the reference list information
    │   └── journal-of-archaeological-science.csl
    |                         # this sets the style of citations & reference list
    ├── figures/
    |
    ├── data/
    │   ├── raw_data/       # data obtained from elswhere
    │   └── derived_data/   # data generated during the analysis
    |
    └──  templates
        ├── template.docx  # used to style the output of the paper.Rmd
        └── template.Rmd

-   The `paper.Rmd` in `analysis/paper/` is ready to render with bookdown
-   The references.bib\` file is empty, ready to insert reference details
-   You can replace the supplied `csl` file with one from <https://github.com/citation-style-language/>
-   we recommend using the [citr addin](https://github.com/crsh/citr) and [Zotero](https://www.zotero.org/) for high efficiency
-   we need to remember that when working in the Rmd writing code, we much update DESCRIPTION Imports: with pkg names used in the Rmd

#### 7. `rrtools::use_dockerfile()`

-   this will create a basic Dockerfile using [`rocker/verse`](https://github.com/rocker-org/rocker) as the base image
-   the version of R in your rocker container will match the version used when you run this function, for example `rocker/verse:3.4.0`
-   [`rocker/verse`](https://github.com/rocker-org/rocker) includes R, the [tidyverse](http://tidyverse.org/), RStudio, pandoc and LaTeX, so build times are very fast
-   we need to edit dockerfile to add linux dependencies (if any) & modify which Rmd files are rendered when the container is made.
-   remember that we need to make an account at <https://hub.docker.com/> to host our Docker container

#### 8. `devtools::use_testthat()`

-   in case we have functions in R/, we need to have some tests to ensure they do what we want
-   Create tests.R in tests/testhat/ and check <http://r-pkgs.had.co.nz/tests.html> for template

You should be able to follow these steps get a new research compendium repository connected to travis and ready to write in just a few minutes.

Future directions
-----------------

-   updating Imports: with `library()`, `require()` and :: calls in the Rmd when `render()`ing

References
----------

Kitzes, J., Turek, D., & Deniz, F. (Eds.). (2017). *The Practice of Reproducible Research: Case Studies and Lessons from the Data-Intensive Sciences*. Oakland, CA: University of California Press. <https://www.practicereproducibleresearch.org>

Marwick, B. (2017). Computational reproducibility in archaeological research: Basic principles and a case study of their implementation. *Journal of Archaeological Method and Theory*, 24(2), 424-450. <https:doi.org/10.1007/s10816-015-9272-9>

Wickham, H. (2017) *Research compendia*. Note prepared for the 2017 rOpenSci Unconf. <https://docs.google.com/document/d/1LzZKS44y4OEJa4Azg5reGToNAZL0e0HSUwxamNY7E-Y/edit#>

Please note that this project is released with a [Contributor Code of Conduct](CONDUCT.md). By participating in this project you agree to abide by its terms.
