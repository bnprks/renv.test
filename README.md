This is repository is a reproducible example of a likely renv bug. In short, renv seems to be confused by nested packages, and even when told to install the subdirectory of the nested package, it will end up executing an installation process for an outer package.

## Code to reproduce problem

A package install from a subdir works fine if nothing "confusing" is in the outer directory: `renv::install("bnprks/renv.test:r@ba11467")`

However, once we toss a `configure` and `DESCRIPTION` file into the root directory installation fails: `renv::install("bnprks/renv.test:r")`

## Why would anyone want nested packages?

The use-case here is the [BPCells repo](https://github.com/bnprks/BPCells/) is switching from being just an R library to holding both an R and a python library. Hence, the R package is now moved to a subdir rather than being at the top level. 

To provide assistance for people still installing from the old location, I've added a minimal package with just `DESCRIPTION` and `configure` files, where the installation will exit with an error message pointing the user to the correct installation location.

This setup works fine with `remotes::install_github`, but it appears to confuse `renv::install` in such a way that only the intentionally broken package installation from the root directory can be run.