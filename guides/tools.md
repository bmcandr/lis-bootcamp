# Useful Programs, Utilities, and Commands

This document provides an overview of programs, utilities, and commands that are useful when working on Discover:

* [diff](#diff) - compare files
* [nccmp](#nccmp-netcdf-compare) - compare NetCDF files
* [vim](#vim-text-editor)


## `diff`



## nccmp - NetCDF Compare

From the [README](https://gitlab.com/remikz/nccmp/-/blob/master/README.md):

>`nccmp` compares two NetCDF files bitwise, semantically or with a user defined tolerance (absolute or relative percentage).  Parallel comparisons are done in local memory without requiring temporary files.  Highly recommended for regression testing scientific models or datasets in a test-driven development environment.
>
>Here's an incomplete list of reasons why you may want to check your data:
>
>* Change in architecture (hardware, bare-metal, virtualization, platform, operating system).
>* Change in compiler, compiler options or dependent libraries.
>* Code refactoring, language migration, algorithm evolution, replacement or optimization.
>* Data type promotion or demotion, compression, shuffling, alignment, ordering, encoding, metadata, or schema change.
>* Process model change (framework, order of execution, partitioning).
>* Tuning model parameters.
>* Up/down-sampling fitness.

Useful flags:

|Flag|Functionality|
|----|-------------|
|`-d`|Compare data|
|`-m`|Compare metadata|
|`-s`|Report identical files|
|`-f`|Force comparison of all attributes (normal behavior is to exit on first difference encountered)
|`-t <threshold>`|Compare if absolute difference is greater than tolerance threshold (e.g., 0.1)

*For a full list of flags and arguments use `nccmp help` or check [this page](https://gitlab.com/remikz/nccmp/-/blob/master/README.md#usage).*


Usage:

```sh
# no tolerance (i.e., check for exact match)
nccmp -dmsf file1.nc file2.nc

# with tolerance of 0.1
nccmp -dmsf -t 0.1 file1.nc file2.nc
```

See [the `nccmp` documentation](https://gitlab.com/remikz/nccmp/-/blob/master/README.md) for more information.

## `vim` - Text Editor

`vim` is a powerful command-line based text editor, but it has a steep learning curve. Here are some resources to get you started:

* Cheatsheets - keyboard commands at a glance
    * [`vim` Cheatsheet 1](https://devhints.io/vim)
    * [`vim` Cheatsheet 2](https://vim.rtorr.com/)
* Interactive `vim` tutorials:
    * [OpenVim](https://www.openvim.com/) - short and sweet introduction
    * [`vim` Adventures](https://vim-adventures.com/) - learn `vim` while playing a game
