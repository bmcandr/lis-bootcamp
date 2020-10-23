# LISF on NCCS Discover - Quick Start Guide

This document provides information to get new users/contributors to LISF up and running on NASA's [Discover](https://www.nccs.nasa.gov/systems/discover) high-performance computing environment.

If you are new to Discover, explore the [user information pages](https://www.nccs.nasa.gov/nccs-users/), [instructional videos](https://www.nccs.nasa.gov/nccs-users/instructional/instructional-videos), and [brown bag presentations](https://www.nccs.nasa.gov/nccs-users/user-events/brown-bag-sessions) available on the NCCS website.

## Contents

* [Accessing Discover](#accessing-discover)
* [Setting up your Environment](#setting-up-your-environment-on-discover)
* [Cloning the LIS Source Code](#cloning-the-lis-source-code)
* [Compiling LIS Components](#compiling-lis-components)
* [Login Nodes vs. Compute Nodes](#login-nodes-vs-compute-nodes)
* [Useful Programs, Utilities, and Commands](#useful-programs-utilities-and-commands)
* [Visualizing Data](visualizing-output.md)

-----

## Accessing Discover

### Prerequisites - What do you need to access Discover?

**Account Prerequisites**

* NASA IdMax identity
* RSA Key
    * Submit [NAMS](https://idmax.nasa.gov/) request *Agency RSA SecurID Token* and follow included instructions
* An account with [NASA Center for Climate Simulation](https://www.nccs.nasa.gov/nccs-users/instructional/account-set-up) (NCCS)
* Discover access permissions (ask your PI)

**Required Software**

Discover is accessed using a [terminal](https://en.wikipedia.org/wiki/Terminal_emulator) application:

* **Mac users:** use *Terminal.app* found in your Applications directory
* **Windows users:** install a terminal emulator such as [PuTTY](https://www.putty.org/) or [MobaXterm](https://mobaxterm.mobatek.net/download.html)

*Need a refresher on terminal/shell basics? [Try this!](http://swcarpentry.github.io/shell-novice/)*

### Connecting to Discover

In the examples that follow, the `%` symbol at the beginning of the line represents the command-line prompt in your terminal. You do not type that when entering any of the example commands. Text following a `>` represents example output that should be returned after running a given command. When copy-pasting into or out of the terminal, you may find that the familiar `Ctrl+C`/`Ctrl+V` keyboard shortcuts do not work. Instead, copy or paste by right clicking on the terminal and selecting the action you'd like to take from the context menu that appears.

1. On your local computer, add the following to your `$HOME/.ssh/config` file:

    ```sh
    Host discover
    #LogLevel Quiet
    User <userid>
    ProxyCommand ssh login.nccs.nasa.gov direct %h
    ForwardX11 yes
    ForwardX11Trusted yes
    Protocol 2
    ```

    Where `<userid>` is your NASA AUID*. This configuration file makes it easier to connect to Discover.

    \**Forgot your AUID? Visit [NAMS](https://nams.nasa.gov) and select Your Identity from the Identities dropdown menu.*

2. Open the terminal* and connect to Discover using ssh:

    ```sh
    % ssh discover
    ```

3. At the `PASSCODE:` prompt, enter the randomly generated 8-digit RSA key that is displayed after entering your PIN in the RSA app.
    * If using an RSA hard token (e.g., key-fob), enter your PIN + 8-digit RSA key at this prompt.

4. At the `Host:` prompt, enter `discover`.

5. At the `Password:` prompt, enter your NCCS LDAP password.

If the connection was successful a welcome message will be printed to the terminal:

```sh
> Your password will expire in X day(s).
> Last login: 1 Jan 1 12:00:00 1970 from login1.nccs.nasa.gov
>
> *******************************************************************************
>
> Please see the NCCS website for current status:
>
> http://www.nccs.nasa.gov/sys.status/motd.html
>
> *******************************************************************************
>
> <userid>@discoverNN:~>
```

At this stage, you are connected to a *login node*. See [Login Nodes vs. Compute Nodes](#login-nodes-vs-compute-nodes) for more information.

NCCS also provides [these instructions](https://www.nccs.nasa.gov/nccs-users/instructional/logging-in/bastion-host) for connecting to their systems.

-----

## Setting up your environment on Discover

You are now connected to Discover. In this section, you will set up your environment on Discover to prepare for compiling and running LISF software.

1. Change directories to your `$HOME` directory:

    ```sh
    % cd $HOME
    ```

2. We suggest that you start with a clean login environment. To do so, move your `.profile` and, assuming you are using bash, `.bashrc` files to another directory for safekeeping. (If your `.bashrc` file does not run any `module load` commands nor sets any critical environment variables like `$PATH` or `$LD_LIBRARY_PATH`, then you should be safe leaving your `.bashrc` file alone.)

    ```sh
    % mkdir profile_temp
    % mv .profile .bashrc profile_temp/
    ```

3. Create a new `.profile` file in your `$HOME` directory...

    ```sh
    % vim .profile
    ```

    ...and add the following to it:

    ```sh
    # This file is read each time a login shell is started.
    if [ -n "$PS1" ]; then
        echo "" ; echo "sourcing clean .profile" ; echo ""
    fi

    module use --append $HOME/privatemodules

    ulimit -s unlimited

    if [ -n "$PS1" ]; then
        echo ""
        echo "sourced clean .profile"
        echo "--------------------"
        echo ""
    fi
    ```

    This file will be executed every time you log onto Discover.

4. Next we need a convenient way to load environment variables that LISF components require to compile and run. To do this we make use of [modules](https://www.nccs.nasa.gov/nccs-users/instructional/using-discover/miscellaneous/using-modules). LISF-specific module files for use on Discover are included alongside the LISF source code in the [`env/discover` directory](https://github.com/NASA-LIS/LISF/tree/master/env/discover) of the GitHub repository. In this step we will download the LISF modulefile for the SLES12 environment to Discover.

    First, make a directory called `privatemodules/` in your `$HOME` directory to store the module files and change into it:

    ```sh
    % mkdir $HOME/privatemodules
    % cd $HOME/privatemodules
    ```

    Using `curl`, download the modulefile for SLES12 into `privatemodules/`:

    ```sh
    % curl -O https://raw.githubusercontent.com/NASA-LIS/LISF/master/env/discover/lisf_7_intel_19_1_0_166
    ```

    *LISF module files are periodically updated as the development environment evolves, check [here](https://github.com/NASA-LIS/LISF/tree/master/env/discover) periodically to ensure you have the latest version.*

    *To learn how to create a custom module file, see [this blog post on ModelingGuru](https://modelingguru.nasa.gov/community/atmospheric/lis/blog/2015/08/26/creating-a-custom-modulefile).*

5. Source your new `.profile` to load your clean environment for this session. This will occur automatically when you login from now on.

    ```sh
    % source $HOME/.profile
    ```

6. Load the LISF modulefile for SLES12 using the `module load` command.

    ```sh
    % module load lisf_7_intel_19_1_0_166
    ```

    The `module` program knows where to find this file no matter where we are in the filesystem because we tell it to look in `$HOME/privatemodules` in the profile created in step 3 (see the line beginning `module use --append`).

    -----

    **Note: you will have to load your modules every time you log onto Discover.**

    -----

7. The storage quota for your `$HOME` directory is pretty small (1GB) so we suggest that you work in your `$NOBACKUP` directory which is located at `/discover/nobackup/<userid>` and has a storage quota of 5GB.

    ```sh
    % cd $NOBACKUP
    ```

    You can check the storage quota in your `$HOME` and `$NOBACKUP` directories by entering the following command:

    ```sh
    % showquota -h

    # the -h flag will show values in "human-friendly" format (i.e., MB and GB rather than KB)
    ```

    The output will also show the storage quota for any additional disks associated with your account.

-----

## Cloning the LISF Source Code

You are now ready to clone the [LISF](https://github.com/NASA-LIS/LISF.git) source code repository to Discover and practice compiling. More information about how the LIS team uses GitHub to collaborate can be found in the [Working with GitHub](https://github.com/NASA-LIS/LISF/blob/master/docs/working_with_github/working_with_github.adoc) documentation.

-----

*New to Git and GitHub? Need a refresher? Try these links:* *[Intro to Git](https://git-scm.com/book/en/v2)* / *[GitHub Guide (Text)](https://help.github.com/en/github)* / *[GitHub Guide (Videos)](https://www.youtube.com/playlist?list=PLg7s6cbtAD15G8lNyoaYDuKZSKyJrgwB-)*

-----

1. In your `$NOBACKUP` directory, create a new directory to work in and change into it. In this example the working directory is called `lis-test`:

    ```sh
    % cd $NOBACKUP
    % mkdir lis-test
    % cd lis-test
    ```

2. Clone the LISF source code:

    ```sh
    % git clone https://github.com/NASA-LIS/LISF.git
    > Cloning into 'LISF'...
    > remote: Enumerating objects: 23, done.
    > remote: Counting objects: 100% (23/23), done.
    > remote: Compressing objects: 100% (23/23), done.
    > remote: Total 11963 (delta 8), reused 4 (delta 0), pack-reused 11940
    > Receiving objects: 100% (11963/11963), 12.98 MiB | 0 bytes/s, done.
    > Resolving deltas: 100% (7547/7547), done.
    > Checking out files: 100% (5615/5615), done.
    ```

    This will download the repository containing the LISF source code into a directory named `LISF/`.

3. Change directories into `LISF/` and use `git status` to check the status of the repository:

    ```sh
    % cd LISF
    % git status
    > On branch master
    > Your branch is up-to-date with 'origin/master'.
    > nothing to commit, working tree clean
    ```

You are now ready to compile the latest LISF source code.

-----

## Compiling LISF Components

This section will provide a brief overview of the process to build the LIS executable from the source code (i.e., compiling). More information can be found in the [LIS User's Guide](https://modelingguru.nasa.gov/docs/DOC-2634) (see *Section 5.4. Build Instructions*). The same process is followed for LDT and LVT.

1. From the `LISF/` directory, change into the `lis/` directory and run the configure script:

    ```sh
    % cd lis
    % ./configure
    ```

    You will be asked to select your compile configuration options. To use the default setting, simply press *Enter*. To use a non-default setting, enter the appropriate option based on the prompt (i.e., 1 to enable or 0 to disable) and press *Enter*. For this exercise, the default settings will suffice.
    Again, more details about these settings may be found in the LISF documentation.

    ```sh
    > Choose the following configure options:
    > Parallelism (0-serial, 1-dmpar, default=1):
    > Optimization level (-3=strict checks with warnings, -2=strict checks, -1=debug, 0,1,2,3, default=2):
    > Assume little/big_endian data format (1-little, 2-big, default=2):
    > Use GRIBAPI/ECCODES? (0-neither, 1-gribapi, 2-eccodes, default=2):
    > Enable AFWA-specific grib configuration settings? (1-yes, 0-no, default=0):
    > Use NETCDF? (1-yes, 0-no, default=1):
    > NETCDF version (3 or 4, default=4):
    # the following netcdf options are set to "off" to speed up writing netcdf files
    > NETCDF use shuffle filter? (1-yes, 0-no, default = 1): 0
    > NETCDF use deflate filter? (1-yes, 0-no, default = 1): 0
    > NETCDF use deflate level? (1 to 9-yes, 0-no, default = 9): 0
    > Use HDF4? (1-yes, 0-no, default=1):
    > Use HDF5? (1-yes, 0-no, default=1):
    > Use HDFEOS? (1-yes, 0-no, default=1):
    > Use MINPACK? (1-yes, 0-no, default=0):
    > Use LIS-CRTM? (1-yes, 0-no, default=0):
    > Use LIS-CMEM? (1-yes, 0-no, default=0):
    > Use LIS-LAPACK? (1-yes, 0-no, default=0):
    ```

2. Compile LIS:

    ```sh
    % ./compile
    ```

    After entering this command you will see a lot of text scrolling by your screen as LIS compiles. This process may take 15-20 minutes and, barring any errors, will result in a file named `LIS`. If you encounter an error, check the LIS documentation and search ModelingGuru for solutions.

    Once you are comfortable with this process, you can speed up compilation by using additional processes. It is recommended that you do this on a compute node by using either an interactive compute node or by submitting a job. See the [NCCS' guidance on Slurm](https://www.nccs.nasa.gov/nccs-users/instructional/using-slurm) for more information. Below is a short example of using an interactive compute node to compile LIS with 16 processes:

    ```sh
    % salloc --ntasks=16 --time=02:00:00
    # Output will appear as resources are allocated and you are connected
    % source ~/.profile  # Source your profile after connecting to an interactive compute node
    % module load lisf_7_intel_19_1_0_166 # Load the LISF module file again
    % cd $NOBACKUP/lis-test/LISF/lis     # Navigate back to the lis directory in the LISF repository
    % ./compile -j 16                   # Compile!
    ```

    *Note: users have been encountering seemingly random compilation failures on SLES12 nodes caused by a `Segmentation Fault`.  Rerunning the `./compile` step typically clears the error. See the [SLES12 Issues](#sles12-issues) section below for more information.*

3. If compilation completes successfully, a file named `LIS` will exist in the `lis/` directory:

    ```sh
    % ls LIS
    > LIS
    ```

You have now compiled the LIS source code to create an exectuable file. The same process is used to build executables for LDT and LVT.

-----

### LISF Test Cases

You are now ready to work through the LISF Public Testcases. This process will verify that your working environment is set up properly by walking you through an end-to-end "experiment" using LDT, LIS, and LVT.

A walkthrough guide for the LISF Public Testcases is available [here](https://github.com/bmcandr/lis-public-tc-walkthrough).

-----

## Login Nodes vs. Compute Nodes

When you first connect to Discover using `ssh` you are placed on a *login node*. **Login nodes** are shared among users and ["intended for basic tasks such as uploading data, managing files, compiling software, editing scripts, and checking on or managing your jobs."](https://wiki.uiowa.edu/display/hpcdocs/Login+Node+Usage#:~:text=The%20login%20nodes%20are%20limited,your%20jobs%20should%20run%20on.) Computationally intensive processes and programs *should not be run on login nodes* because it may affect other users sharing your node. Instead, processes that consume large amounts of memory and/or processing power should be run on a **compute node.**

**Compute nodes** can be accessed interactively, similar to working on a login node, or jobs can be submitted to run on compute nodes in the background using `slurm`. To learn more about using compute nodes visit the NCCS instructional page for [using slurm](https://www.nccs.nasa.gov/nccs-users/instructional/using-slurm).

-----

## Useful Programs, Utilities, and Commands

This document provides an overview of programs, utilities, and commands that are useful when working on Discover:

* [diff](#diff---compare-files) - compare files
* [nccmp](#nccmp---compare-netcdf-files) - compare NetCDF files
* [slurm](#slurm---batch-queue-utility) - job scheduler
* [vim](#vim---text-editor) - text editor
* [emacs](#emacs---text-editor) - text editor

-----

### `diff` - Compare Files

The `diff` utility compares files line-by-line. This is useful to quickly compare configuration files, logs, or output containing descriptive statistics (e.g., SURFACEMODEL.d01.stats).

Usage:

```sh
diff file1.txt file2.txt
```

*Note: If the input files are identical, no output will be printed to the terminal unless the `-s` flag is used.*

Use the `--help` flag to learn more.

-----

### `nccmp` - Compare NetCDF Files

From the official [README](https://gitlab.com/remikz/nccmp/-/blob/master/README.md):

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
|`-f`|Forcefully compare, do not stop after first difference|
|`-t <threshold>`|Compare if absolute difference is greater than tolerance threshold (e.g., 0.1)|
|`-S` or `--statistics`|Reports statistics for any differences in data values|
|`-F` or `--fortran`|Print position indices using Fortran style (1-based reverse order)

*For a full list of flags and arguments use `nccmp --help` or [click here](https://gitlab.com/remikz/nccmp/-/blob/master/README.md#usage).*

Usage:

```sh
# no tolerance (i.e., check for exact match)
nccmp -dmsf file1.nc file2.nc

# with tolerance of 0.1
nccmp -dmsf -t 0.1 file1.nc file2.nc
```

See [the `nccmp` documentation](https://gitlab.com/remikz/nccmp/-/blob/master/README.md) for more information.

**Discover users** on can access `nccmp` on *SLES12* nodes by loading the appropriate module:

```sh
module load nccmp
```

-----

### `slurm` - Job Scheduler

* [NCCS' Intro to `slurm`](https://www.nccs.nasa.gov/nccs-users/instructional/using-slurm)
* [Official Documentation](https://slurm.schedmd.com/documentation.html)

-----

### Text Editors

* `vim`

    `vim` is a powerful command-line based text editor, but it has a steep learning curve. Here are some resources to get started:

    * Cheatsheets - keyboard commands at a glance
        * [`vim` Cheatsheet 1](https://devhints.io/vim)
        * [`vim` Cheatsheet 2](https://vim.rtorr.com/)
    * Interactive `vim` tutorials:
        * [OpenVim](https://www.openvim.com/) - short and sweet introduction
        * [`vim` Adventures](https://vim-adventures.com/) - learn `vim` while playing a game

* `emacs`

    `emacs` is another powerful text editor available from the command-line that is known for its customizability. A built-in tutorial can be accessed by starting `emacs` and typing `Ctrl+h` followed by `t`.

-----

### SLES12 Issues

This section describes a few known issues you may encounter when working with LISF on SLES12 nodes.

1. Segmentation fault during compilation

    Users are encountering seemingly random segmentation fault errors during the compilation step. The error message will look similar to this:

    ```sh
    /usr/local/intel/2020/compilers_and_libraries_2020.0.166/linux/mpi/intel64/bin/mpiicc: line 558:  6248 Segmentation fault      (core dumped) /usr/local/intel/2020/compilers_and_libraries_2020.0.166/linux/bin/intel64/icc -c -g '-traceback' '-DIFC' '-DLINUX' '-DLIS_JULES' '-I./' '-I../core/' '-I../params/gfrac/SPORTDaily/' '-I../params/gfrac/VIIRSDaily/' '-I../surfacemodels/land/vic.4.1.1/' '-I../surfacemodels/land/vic.4.1.1/physics/' '-I../surfacemodels/land/vic.4.1.2.l/' '-I../surfacemodels/land/vic.4.1.2.l/physics/' '-I../surfacemodels/land/rdhm.3.5.6/' '-I../surfacemodels/land/rdhm.3.5.6/sac/' '-I../surfacemodels/land/rdhm.3.5.6/frz/' '-I../surfacemodels/land/rdhm.3.5.6/snow17/' '-I../surfacemodels/land/rdhm.3.5.6/sachtet/' '../surfacemodels/land/rdhm.3.5.6/sac/days_of_month.c' -I/usr/local/intel/2020/compilers_and_libraries_2020.0.166/linux/mpi/intel64/include
    Makefile:145: recipe for target 'days_of_month.o' failed
    make[1]: *** [days_of_month.o] Error 139
    make[1]: Leaving directory '/gpfsm/dnb31/dcrosen/src/lishydro_dev/src/LISF/lis/make'

    [ERR] Compile failed
    ```

    We are currently working with NCCS to resolve this issue. For now, we recommend that you re-run the compile step (i.e., `./compile`). This will usually allow compilation to finish.

2. Running LDT and LVT on SLES12 login nodes

    You cannot run MPI-based executables on the SLES12 *login* nodes, even if you are just using a single process. On SLES12, you must run MPI-based executables on a *compute* node, either interactively (i.e., `xalloc`) or via batch (i.e., `sbatch`).

    This is because the `lisf_7_intel_19_1_0_166` modulefile sets environment variables that point to MPI-based compilations of ESMF. So even if you select "serial" at the parallelization prompt, ESMF will bring in MPI which leads to the error below:

    ```sh
    $ ./LDT ldt.config
    Abort(2140047) on node 0 (rank 0 in comm 0): Fatal error in PMPI_Init_thread: Other MPI error, error stack:
    MPIR_Init_thread(703)........:
    MPID_Init(958)...............:
    MPIDI_OFI_mpi_init_hook(1334):
    MPIDU_bc_table_create(444)...:
    ```

    There are two options for compiling and running LDT and LVT:

    1. Compile the executables with the `lisf_7_intel_19_1_0_166` modulefile and *always* run on a compute node (see the [NCCS Slurm guidance](https://www.nccs.nasa.gov/nccs-users/instructional/using-slurm)). *This is the recommended approach.*
    2. Copy the contents of the `lisf_7_intel_19_1_0_166` modulefile into new modulefile named `lisf_7_intel_19_1_0_166_mpiuni`. Then, edit the definitions (i.e., path) for `def_lis_modesmf` and `def_lis_libesmf` to change `intelmpi` to `mpiuni` as shown here:

        ```sh
        set   def_lis_modesmf     /discover/nobackup/projects/lis/libs/esmf/7.1.0r_intel-19.1.0.166_impi-20.0.0.166_sles12.3/mod/modO/Linux.intel.64.mpiuni.default
        set   def_lis_libesmf     /discover/nobackup/projects/lis/libs/esmf/7.1.0r_intel-19.1.0.166_impi-20.0.0.166_sles12.3/lib/libO/Linux.intel.64.mpiuni.default
        ```

        Load the `lisf_7_intel_19_1_0_166_mpiuni` modulefile whenever compiling and running LDT and LVT on a login node.

    *Be careful when using option 2.* When using option 2, you must remember to load `lisf_7_intel_19_1_0_166_mpiuni` when compiling LDT and LVT, but you must unload this modulefile and load `lisf_7_intel_19_1_0_166` to compile and run LIS. Use `module list` to check which module files are currently loaded and `module unload <modulefile name>` or `module purge` to unload one or all currently loaded modules.
