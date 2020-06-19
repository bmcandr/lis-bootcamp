# Visualizing LIS Output

This guide will demonstrate several approaches to visualizing output from LIS framework components. There are many different visualization tools available on NCCS Discover, but we will focus our attention on the following tools:

* [GrADS](#grads)
* [Python](#python)

## GrADS

Several of us in the lab like using GrADS because it is quick and handles binary, NetCDF, and GRIB data relatively easy.  It is good for both interactively viewing data as well as scripting for batch processing. Others find GrADS awkward to use or find the output to be ugly. They use other tools like Matlab, Python/matplotlib, or NCL, for visualizing data. We suggest starting with GrADS because all our existing testcases have GrADS descriptor files (*.ctl) to help with plotting the data.

*Note: This section will help you get GrADS running on Discover, but should not be considered a comprehensive reference. Please see the [GrADS Documentation](http://cola.gmu.edu/grads/gadoc/gadoc.php) for detailed descriptions of GrADS functionality and usage.*

First you must set two environment variables so GrADS can find its fonts and supplemental scripts. This should work, and it is safe to put into your `.profile` in your `$HOME`:

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

To run GrADS by simply entering `grads` in the terminal, create an alias in your `.profile` to point to your preferred version. For example:

```sh
...
alias grads=$NOBACKUP/../projects/lis/libs/grads/2.1.a2/bin/grads
...
```

GrADS uses descriptor files with the extension .ctl and .xdf to create plots. To generate a plot from these files enter a command such as:

```sh
grads -lc "open <filename>.ctl
```

Where the `l` flag sets GrADS to plot in landscape layout and the `c` flag tells GrADS to run the command in quotes on execution.

If no visualization appears, one of two things are happening.

1. Your X11 forwarding is not working.  Try running `xclock`.  If clock pops up on your local computer, the X11 forwarding is working.
2. You did not actually tell GrADS to plot anything.

....
grads -lc "open test_out.ctl"
set gxout grfill
q file
d mask
....

`set gxout grfill` tells GrADS to draw full color plot instead of contour lines.  `q file` lists all the fields in the file and should only list "mask." `d mask` tells GrADS to display the mask field.

You may see the following *warning*, but it is safe to ignore:

```sh
Warning: OPTIONS keyword "template" is used, but the
DSET entry contains no substitution templates.
```

-----

## R

1. Load R module:

    ```sh
    module load R       # loads default R version
    ```

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

### Installing Miniconda

This section describes how to install [Miniconda](https://docs.conda.io/en/latest/miniconda.html), a minimal installation of the [Anaconda](https://www.anaconda.com/) package manager, on NCCS Discover.

1. Connect to Discover.

2. Change directories into your `$NOBACKUP` directory:

    ```sh
    % cd $NOBACKUP
    ```

3. Use `curl` to download the install script to Discover.

    * Python 3.7:

        ```sh
        % curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
        ```

    * Python 2.7:

        ```sh
        % curl -O https://repo.anaconda.com/miniconda/Miniconda2-latest-Linux-x86_64.sh
        ```

    *Note: visit the Miniconda website for links to other installers.*

4. Run the install script:

    ```sh
    % bash Miniconda-install-script.sh    # change filename to name of script downloaded
    ```

    At the prompt asking where to install, enter:

    ```sh
    $NOBACKUP/miniconda3        #use $NOBACKUP/miniconda2 if installing Python 2.7 version
    ```

5. After the installer runs the terminal will ask if you would like to initialize `conda` for your shell. Select *No*.

6. Create a file in your `$HOME` directory called `.conda_profile`:

    ```sh
    % touch ~/.conda_profile
    ```

    Use `vim` to add the following to this file (e.g., `vim ~/.conda_profile`), replacing `<user_id>` with your NASA AUID:

    ```
    # >>> conda initialize >>>
    # !! Contents within this block are managed by 'conda init' !!
    __conda_setup="$('$NOBACKUP/<user_id>/miniconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
    if [ $? -eq 0 ]; then
        eval "$__conda_setup"
    else
        if [ -f "$NOBACKUP/<user_id>/miniconda3/etc/profile.d/conda.sh" ]; then
            . "$NOBACKUP/<user_id>/miniconda3/etc/profile.d/conda.sh"
        else
            export PATH="$NOBACKUP/<user_id>/miniconda3/bin:$PATH"
        fi
    fi
    unset __conda_setup
    # <<< conda initialize <<<
    ```

7. Miniconda is now installed. To initialize, source the `.conda_profile` file created in the previous step:

    ```sh
    % bash ~/.conda_profile
    ```

    **NOTE: this intialization step must be run every time you log onto Discover, jump to a SLES11 node, or start an interactive session on a compute node (e.g., `salloc` or `xalloc`). To initialize `conda` automatically on login, you can add the command above to your `.profile` file however this may have unintended affects as it modifies your $PATH variable.**

8. Activate `conda`:

    ```sh
    % conda activate
    ```

    If successful, `(base)` will appear at the start of your terminal prompt. For example:

    ```sh
    (base) <userid>@discoverNN:/discover/nobackup/userid>
    ```

    This indicates that the base environment of `conda` is activated. See the [Anaconda Documentation](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html) to learn about Anaconda virtual environments.

    You can check your Python path to verify that the it is pointing to the one installed with Miniconda:

    ```sh
    % which python
    > /discover/nobackup/<userid>/miniconda3/bin/python
    ```

### Visualizing Data with Python

This section is under construction...
