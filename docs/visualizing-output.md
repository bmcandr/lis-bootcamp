# Visualizing LIS Output on Discover

This guide will demonstrate several approaches to visualizing output from LIS framework components. There are many different visualization tools available on NCCS Discover, but we will focus our attention on the following tools:

* [GrADS](#grads)
* [R](#r)
* [Python](#python)

## GrADS

Several of us in the lab like using [GrADS](http://cola.gmu.edu/grads/) because it is quick and handles binary, NetCDF, and GRIB data relatively easy. It is good for both interactively viewing data as well as scripting for batch processing. Others find GrADS awkward to use or find the output to be ugly. They use other tools like Matlab, Python/matplotlib, or NCL for visualizing data. We suggest starting with GrADS because all our existing testcases have GrADS descriptor files (*.ctl) to help with plotting the data.

This section will help you get GrADS running on Discover, but should not be considered a comprehensive reference. Please visit the GrADS website for [Official Documentation](http://cola.gmu.edu/grads/gadoc/gadoc.php) and an [introductory tutorial](http://cola.gmu.edu/grads/gadoc/tutorial.html).

### Setting up GrADS

On SLES12, the default environment on login to Discover, simply add GrADS to your environment path as follows:

```sh
% export PATH=$PATH:/discover/nobackup/projects/gmao/share/dasilva/opengrads/Contents
```

You will have to re-run the above command each time you log onto Discover. To automatically add GrADS to your path on login, add the following line to your `~/.profile` file:

```sh
PATH=$PATH:/discover/nobackup/projects/gmao/share/dasilva/opengrads/Contents
```

Run the command `grads` to start the program. If successful a welcome message will print to the terminal and an X11 window will open.

```sh
% grads -l
>
>              Welcome to the OpenGrADS Bundle Distribution
>              --------------------------------------------
>
>For additional information enter "grads --manual".
...
```

*Note: In the snippet above, the `-l` flag tells GrADS to plot in landscape layout. If you do not pass this flag, GrADS will ask you to specify portrait or landscape mode on startup (use `y` or `n` to answer).*

Your terminal prompt will now be prefixed with `ga->`, signifying that GrADS is running. This is where you will enter commands to GrADS.

To exit and return to the shell prompt, simply enter `quit` at the GrADS prompt.

### Intro to Plotting LIS Input/Output

*The examples below use files associated with Step 2 of the [LISF Public Testcase](https://github.com/bmcandr/lis-public-tc-walkthrough). Download the files [here](https://portal.nccs.nasa.gov/lisdata_pub/Tutorials/Web_Version/testcase2_lis_ol.tgz)*

GrADS uses text-based "descriptor files" with the extension `.ctl` and `.xdf` to create plots. To open a descriptor file named `output.ctl` for example, enter the `open` command followed by the filename:

```sh
ga-> open output.ctl
```

For `.xdf` files, such as `target_da.xdf` included with the Step 6 files, use the command `xdfopen`:

```sh
ga-> xdfopen target_da.xdf
```

This file can now be referenced in GrADS as `file`. Query the contents using the `q` command:

```sh
ga-> q file
File 1 : LIS land surface model output
  Descriptor: target_ol.xdf
  Binary: ./target_OL_OUTPUT/SURFACEMODEL/2017%m2/LIS_HIST_2017%m2%d2%h200.d01.nc
  Type = Gridded
  Xsize = 28  Ysize = 22  Zsize = 4  Tsize = 1460  Esize = 1
  Number of Variables = 20
     lat  0  y,x  latitude
     lon  0  y,x  longitude
     swnet_tavg  0  y,x  net downward shortwave radiation
     lwnet_tavg  0  y,x  net downward longwave radiation
     qle_tavg  0  y,x  latent heat flux
     qh_tavg  0  y,x  sensible heat flux
     qg_tavg  0  y,x  soil heat flux
     snowf_tavg  0  y,x  snowfall rate
     rainf_tavg  0  y,x  precipitation rate
     evap_tavg  0  y,x  total evapotranspiration
     qs_tavg  0  y,x  surface runoff
     qsb_tavg  0  y,x  subsurface runoff amount
     albedo_tavg  0  y,x  surface albedo
     swe_tavg  0  y,x  snow water equivalent
     snowdepth_tavg  0  y,x  snow depth
     soilmoist_tavg  4  z,y,x  soil moisture content
     rainf_f_tavg  0  y,x  rainfall flux
     tair_f_tavg  0  y,x  air temperature
     swdown_f_tavg  0  y,x  surface downward shortwave radiation
     lwdown_f_tavg  0  y,x  surface downward longwave radiation
```

The variables listed in the output can be plotted using the `display`, or `d`, command followed by the variable name:

```sh
ga-> set gxout grfill   # change plot style from contour to fill (grfill)
ga-> d soilmoist_tavg  # plot the soilmoist_tavg1 for the first timestep
```

<img src='images/grads-soilmoist-no-cbar-example-plot.png' width=75%>

Add a colorbar with `cbar`:

```sh
ga-> cbar
```

<img src='images/grads-soilmoist-example-plot.png' width=75%>

Clear the output using the `clear` or `c` command:

```sh
ga-> c
```

To view the file at the 20th timestep (timesteps are defined in the descriptor file) use the `set` command followed by the `d` command:

```sh
ga-> set t 20
ga-> d soilmoist_tavg
```

<img src='images/grads-soilmoist-T20-example-plot.png' width=75%>

For more examples, check out the [GrADS Official Tutorial](http://cola.gmu.edu/grads/gadoc/tutorial.html).

-----

### Troubleshooting

If no visualization appears, one of two things are happening.

1. Your X11 forwarding is not working.  Try running `xclock`.  If clock pops up on your local computer, the X11 forwarding is working.
2. You did not actually tell GrADS to plot anything.

You may see the following *warning*, but it is safe to ignore:

```sh
Warning: OPTIONS keyword "template" is used, but the
DSET entry contains no substitution templates.
```

-----

### Legacy Instructions for the SLES11 Environment

To use GrADS on SLES11 nodes, you must set two environment variables so GrADS can find its fonts and supplemental scripts. The following commands should work and are safe to add to your `~/.profile`:

```sh
# place after existing entries

export GADDIR=/home/jvgeiger/local/opengrads-2.0.2.oga.2/data
export GASCRP=/home/jvgeiger/SRC/GrADS

# and before "echo sourced clean .profile"
```

There are two versions of GrADS available and they are located at:

|Version|Location|
|-------|--------|
|2.0.1|`$NOBACKUP/../projects/lis/libs/grads/2.0.1/bin/grads`
|2.1.a2|`$NOBACKUP/../projects/lis/libs/grads/2.1.a2/bin/grads`

To run GrADS by simply entering `grads` in the terminal, create an alias in your `~/.profile` to point to your preferred version. For example:

```sh
alias grads=$NOBACKUP/../projects/lis/libs/grads/2.1.a2/bin/grads
```

-----

## R

1. Load the appropriate LISF module for your node (e.g., SLES11 or SLES12) and then load the R module:

    ```sh
    module load lisf_7_intel_19_1_0_166
    module load R       # loads default R version
    ```

    Loading the `lisf_7...` module will set environmental variables needed by R to install additional packages.

    *Note: multiple versions of R are available. Use `module avail` to view all available versions.*

2. Packages need to be downloaded to a local directory.  Sample R script for downloading packages:

    ```R
    #!/usr/bin/env Rscript

    pkg=c("rgeos", "rgdal", "sp", "raster”)
    install.packages(pkg, lib="$NOBACKUP/rPkg2", repos='https://cloud.r-project.org', type='source')
    ```

    *Note: the order that packages are installed matters – dependencies must be installed first (e.g., sp must be installed before raster).  Also note that the rgdal package **must** be installed with the type=’source’ flag.*

3. Sample R script with package:

    ```R
    #!/usr/bin/env Rscript

    library(ncdf4, lib.loc="$NOBACKUP/rPkg")

    d='20200524'

    NoahMP=nc_open('LIS_RST_NOAHMP401_200909302330.d01.nc')
    Noah36=nc_open('LIS_RST_NOAH36_modis2_200910010000.d01.nc')
    old='LIS_RST_NOAHMP401_200909302330.d01.nc'
    new=paste0('LIS_RST_NOAHMP401_HACK.d01_', d, '.nc')
    print(paste0('writing new file ', new))
    file.copy(old, new, overwrite=T)
    ```

    *Note that the order that packages are loaded matters – dependencies must be loaded first (e.g., sp must be loaded before raster).*

-----

## Python

Check this document: <https://modelingguru.nasa.gov/docs/DOC-2693>

**The information below is inaccurate and will be updated eventually.**

### Loading an Anaconda distribution of Python

The NCCS Discover environment provides access to several versions of Python through the [Anaconda](https://www.anaconda.com/) distribution/package manager. These can be viewed and loaded using the `module` program. To view a list of available Anaconda distributions, use the `module avail` command. As of July 2020, the following modules are available:

* `python/GEOSpyD/Ana2018.12_py2.7`
* `python/GEOSpyD/Ana2018.12_py3.7`
* `python/GEOSpyD/Ana2019.03_py2.7`
* `python/GEOSpyD/Ana2019.03_py3.7`
* `python/GEOSpyD/Ana2019.10_py2.7`
* `python/GEOSpyD/Ana2019.10_py3.7 (D)`

To load one of the above distributions and add the associated Python installation to your `$PATH`, use the `module load` command. For example, to load the latest version enter the following command:

```sh
% module load python/GEOSpyD/Ana2019.10_py3.7
```

To verify that your `$PATH` has been modified to point to the loaded Python installation enter the command `which python`:

```sh
% which python
> /usr/local/other/python/GEOSpyD/2019.10_py3.7/2020-01-15/bin/conda
```

Additional usage information can be found here: <https://modelingguru.nasa.gov/docs/DOC-2693>

### Enabling Miniconda

Another option is to utilize an installation of `miniconda`, a minimal/lightweight installation of the Anaconda distribution. This section describes how to enable [Miniconda](https://docs.conda.io/en/latest/miniconda.html), a minimal installation of the [Anaconda](https://www.anaconda.com/) package manager, in the Discover environment.

1. On Discover, create a file in your `$HOME` directory called `.conda_profile`:

    ```sh
    % touch ~/.conda_profile
    ```

    Use `vim` to add the following to this file (e.g., `vim ~/.conda_profile`):

    ```sh
    # >>> conda initialize >>>
    # !! Contents within this block are managed by 'conda init' !!
    __conda_setup="$('/usr/local/other/miniconda/4.7.12/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
    if [ $? -eq 0 ]; then
        eval "$__conda_setup"
    else
        if [ -f "/usr/local/other/miniconda/4.7.12/etc/profile.d/conda.sh" ]; then
            . "/usr/local/other/miniconda/4.7.12/etc/profile.d/conda.sh"
        else
            export PATH="/usr/local/other/miniconda/4.7.12/bin:$PATH"
        fi
    fi
    unset __conda_setup
    # <<< conda initialize <<<
    ```

2. To add Miniconda to your path so you can run it from the command line, source the `.conda_profile` file created in the previous step:

    ```sh
    % source ~/.conda_profile
    ```

    *NOTE: this intialization step must be run every time you log onto Discover, jump to a SLES11 node, or start an interactive session on a compute node (e.g., `salloc` or `xalloc`). To initialize `conda` automatically on login, you can add the command above to your `.profile` file however this may have unintended affects as it modifies your $PATH variable.*

3. Activate `conda`:

    ```sh
    % conda activate
    ```

    If successful, `(base)` will appear at the start of your terminal prompt. For example:

    ```sh
    (base) <userid>@discoverNN:~>
    ```

    This indicates that the base environment of `conda` is activated. See the [Anaconda Documentation](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html) to learn about Anaconda virtual environments.

    Use `which python` to check that your Python path now points to the Miniconda installation:

    ```sh
    % which python
    > /usr/local/other/miniconda/4.7.12/bin/python
    ```

It is *strongly* recommended that users employ virtual environments to manage their Python environments. Python versions 3.3+ include virtual environment functionality as described [here](https://docs.python.org/3/library/venv.html), while Anaconda/Miniconda provide their own virtual environment functionality described [here](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html). Both do the same thing, but in slightly different ways.

<!-- 
### Visualizing Data with Python

This section is under construction... -->
