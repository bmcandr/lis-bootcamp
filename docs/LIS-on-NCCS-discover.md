# Getting Started with LIS on NCCS Discover

This document provides information to get new users/contributors to LIS up and running on NASA's [Discover](https://www.nccs.nasa.gov/systems/discover) high-performance computing environment. We recommend reviewing the [user information](https://www.nccs.nasa.gov/nccs-users/) and [instructional videos](https://www.nccs.nasa.gov/nccs-users/instructional/instructional-videos) available on the NCCS website before continuing with this guide.

## Contents

* [Accessing Discover](#accessing-discover)
* [Setting up your Environment](#setting-up-your-environment-on-discover)
* [Cloning the LIS Source Code](#cloning-the-lis-source-code)
* [Compiling LIS Components](#compiling-lis-components)
* [Login Nodes vs. Compute Nodes](#login-nodes-vs-compute-nodes)
* [Useful Programs, Utilities, and Commands](#useful-programs-utilities-and-commands)
* [Visualizing Data](visualizing-data.md)

-----

## Accessing Discover

### Prerequisites

What do you need to access Discover?

* NASA IdMax identity
* RSA Key
    * Submit [NAMS](https://idmax.nasa.gov/) request *Agency RSA SecurID Token* and follow included instructions
* An account with [NASA Center for Climate Simulation](https://www.nccs.nasa.gov/nccs-users/instructional/account-set-up) (NCCS)
* Discover permissions (ask your PI)
* Familiarity with using the terminal or shell (e.g., bash)
    * Need a refresher? [Try this!](http://swcarpentry.github.io/shell-novice/)

### Connecting to Discover

In the examples that follow, the `%` symbol at the beginning of the line represents the command-line prompt. You do not type that when entering any of the commands. Text following a `>` represents example output that should be returned after running a given command. When copy-pasting into or out of the terminal, you may find that the familiar `Ctrl+C`/`Ctrl+V` keyboard shortcuts do not work. Instead, copy or paste by right clicking on the terminal and selecting the action you'd like to take from the context menu that appears.

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

    You will be connected to a login node running the SLES12 operating system.

    * To jump to one of the older SLES11 login nodes enter the following command while connected to Discover:

        ```sh
        % ssh -Y discover-sles11
        ```

    \**Windows users will have to install a terminal emulator such as [PuTTY](https://www.putty.org/) or [MobaXterm](https://mobaxterm.mobatek.net/download.html)*.

3. At the `PASSCODE:` prompt, enter the randomly generated 8-digit RSA key that is displayed after entering your PIN in the RSA app.

4. At the `Host:` prompt, enter `discover`.

5. At the `Password:` prompt, enter your NCCS LDAP password.

If the connection was successful a welcome message will be printed to the terminal:

```
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

NCCS also provides [these instructions](https://www.nccs.nasa.gov/nccs-users/instructional/logging-in/bastion-host) for connecting to their systems.

At this stage, you are connected to a *login node*. See [Login Nodes vs. Compute Nodes](#login-nodes-vs-compute-nodes) for more information.

-----

## Setting up your environment on Discover

1. Change directories to your `$HOME` directory:

    ```sh
    % cd $HOME
    ```

2. We suggest that you start with a clean login environment. To do so, move your `.profile` and, assuming you are using bash, `.bashrc` files to another directory for safekeeping. If your `.bashrc` file does not run any `module load` commands nor sets any critical environment variables like `$PATH` or `$LD_LIBRARY_PATH`, then you should be safe leaving your `.bashrc` file alone.

    ```sh
    % mkdir profile_temp
    % mv .profile .bashrc profile_temp/
    ```

3. Create a new `.profile` file in your `$HOME` directory...

    ```sh
    % vim .profile
    ```

    ...and add the following to it:

    <!-- replace with David's autoloader for SLES11/12? -->

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

    This file will be executed every time you log onto Discover to ensure you're working in a clean environment.

    Since you've just created your new `.profile`, you'll need to execute the following command to load your clean environment for this session:

    ```sh
    % source $HOME/.profile
    ```

4. Now that you have a clean environment, environment variables that LIS needs to compile and run must be loaded. To do this we will make use of [modules](https://www.nccs.nasa.gov/nccs-users/instructional/using-discover/miscellaneous/using-modules). Module files created for use with LIS are available in the LISF GitHub repo in the [env/discover](https://github.com/NASA-LIS/LISF/tree/master/env/discover) directory. In this step we will download these module files to Discover.

    Make a directory called `privatemodules/` in your `$HOME` directory to store the module files:

    ```sh
    % mkdir $HOME/privatemodules
    ```

    Then, change directories into `privatemodules/`:

    ```sh
    % cd $HOME/privatemodules
    ```

    Using `curl`, download the modulefiles for SLES11 and SLES12 into `privatemodules/`:

    * **SLES12:**

        ```sh
        % curl -O https://raw.githubusercontent.com/NASA-LIS/LISF/master/env/discover/lisf_7_intel_19_1_0_166
        ```

    * **SLES11:**

        ```sh
        % curl -O https://raw.githubusercontent.com/NASA-LIS/LISF/master/env/discover/lisf_7_intel_18_0_3_222
        ```

    <!-- add GNU module file? add mpiuni files?-->

    *Check [here](https://github.com/NASA-LIS/LISF/tree/master/env/discover) periodically for updated module files.*

5. Load the appropriate modulefile using the `module load` command.

    * On **SLES12\*:**

        ```sh
        % module load lisf_7_intel_19_1_0_166
        ```

        \**You may encounter issues compiling and running LIS on SLES12. Please see [the SLES12 Issues section](#sles12-issues) for potential solutions.*

    * On **SLES11:**

        ```sh
        % module load lisf_7_intel_18_0_3_222
        ```

    -----

    ***Note: you will have to load the appropriate module every time you log onto Discover!***

    -----

6. The storage quota for your `$HOME` directory is pretty small so we suggest that you work in your `$NOBACKUP` directory which is located at `/discover/nobackup/<userid>`.

    ```sh
    % cd $NOBACKUP
    ```

    You can check the storage quota in your `$HOME` and `$NOBACKUP` directories by entering the following command:

    ```sh
    % /usr/local/bin/showquota

    # Note: Values will be shown in kilobytes (KB). Divide by 1000 to convert to megabytes or 10000 to convert to gigabytes
    ```

-----

## Cloning the LIS Source Code

You are now ready to clone the [LISF](https://github.com/NASA-LIS/LISF.git) source code repository and practice compiling. More information about how the LIS team collaborates can be found in the [Working with GitHub](https://github.com/NASA-LIS/LISF/blob/master/docs/working_with_github/working_with_github.adoc) documentation.

*New to Git and GitHub? Need a refresher? Try these links:* *[Intro to Git](https://git-scm.com/book/en/v2)* / *[GitHub Guide (Text)](https://help.github.com/en/github)* / *[GitHub Guide (Videos)](https://www.youtube.com/playlist?list=PLg7s6cbtAD15G8lNyoaYDuKZSKyJrgwB-)*

1. In your `$NOBACKUP` directory, create a new directory to work in:

    ```sh
    % cd $NOBACKUP
    % mkdir temp
    % cd temp/
    ```

2. Load Git using `module`:

    ```sh
    % module load git         # SLES 12
    # OR
    % module load other/git   # SLES 11
    ```

    *[Click here](https://www.nccs.nasa.gov/nccs-users/instructional/using-discover/miscellaneous/using-modules) more information on using modules.*

3. Clone the LISF source code:

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

    This will download the [Git repository](https://github.com/NASA-LIS/LISF.git) containing the LIS source code into a directory named `LISF/`.

4. Change directories into `LISF/` and check the status of the repository:

    ```sh
    % cd LISF
    % git status
    > On branch master
    > Your branch is up-to-date with 'origin/master'.
    > nothing to commit, working tree clean
    ```

You are now ready to practice compiling.

-----

## Compiling LIS Components

This section will provide a brief overview of the process to build the LIS executable from the source code (i.e., compiling). More information can be found in the [LIS Users' Guide](https://modelingguru.nasa.gov/docs/DOC-2634) (See *Section 5.4. Build Instructions*).

1. From the `LISF/` directory, run the configure script inside the `lis/` directory:

    ```sh
    % cd lis
    % ./configure
    ```

    You will then be asked to select your configuration options. Note that for some settings, the default option is sufficient and you can simply press *Enter*. For others, you may want to select another option (e.g., NetCDF settings). Again, more details about these settings may be found in the LIS documentation.

    ```sh
    > Choose the following configure options:
    > Parallelism (0-serial, 1-dmpar, default=1):
    > Optimization level (-3=strict checks with warnings, -2=strict checks, -1=debug, 0,1,2,3, default=2):
    > Assume little/big_endian data format (1-little, 2-big, default=2):
    > Use GRIBAPI/ECCODES? (0-neither, 1-gribapi, 2-eccodes, default=1): 2
    > Enable AFWA-specific grib configuration settings? (1-yes, 0-no, default=0):
    > Use NETCDF? (1-yes, 0-no, default=1):
    > NETCDF version (3 or 4, default=4):
    # the following netcdf compression flags are turned off to speed up writing netcdf files
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

    *Note that the configurations settings you select may differ based on the module you have loaded.*

2. Compile LIS:

    ```sh
    % ./compile
    ```

    After entering this command you will see a lot of text scrolling by your screen as LIS compiles. This process may take 15-20 minutes and, barring any errors, will result in a file named `LIS`. If you encounter an error, check the LIS documentation and search ModelingGuru for solutions.

    Once you are comfortable with this process, you can speed up the compilation process by modifying the above command to utilize more jobs:

    ```sh
    % ./compile -j 16
    ```

    *Note: users have been encountering seemingly random compilation failures caused by a `Segmentation Fault` on SLES12.  Rerunning the `./compile` step typically clears the error. See the [SLES 12 Issues](#sles12-issues) section below for more information.*

3. Move the `LIS` executable into your working directory:

    ```sh
    % mv LIS ../../   # moves the exectuable up two directories (out of the LISF/ repo)
    ```

You have now compiled the LIS source code to create an exectuable file. The same process is used to build executables for LDT and LVT.

-----

### LIS Test Cases

You are now ready to work through the LIS test cases. This process will verify that your working environment is set up properly and walk through the process to configure, compile, and run LDT, LIS, and LVT. Instructions and files needed to run the test cases can be found [here](https://lis.gsfc.nasa.gov/tests/lis). Additional tips are included [here](LIS-testcases.md).

*Be sure to complete each step of the test cases in order because later steps take output from earlier steps as input.*

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

    -----

    ### `slurm` - Job Scheduler

    * [NCCS' Intro to `slurm`](https://www.nccs.nasa.gov/nccs-users/instructional/using-slurm)
    * [Official Documentation](https://slurm.schedmd.com/documentation.html)

    -----

    ### `vim` - Text Editor

    `vim` is a powerful command-line based text editor, but it has a steep learning curve. Here are some resources to get you started:

    * Cheatsheets - keyboard commands at a glance
        * [`vim` Cheatsheet 1](https://devhints.io/vim)
        * [`vim` Cheatsheet 2](https://vim.rtorr.com/)
    * Interactive `vim` tutorials:
        * [OpenVim](https://www.openvim.com/) - short and sweet introduction
        * [`vim` Adventures](https://vim-adventures.com/) - learn `vim` while playing a game

-----

### SLES12 Issues

***NOTE: This section is under construction***

We have been having issues running on SLES12; in particular:

```sh
$ ./LDT ldt.config
Abort(2140047) on node 0 (rank 0 in comm 0): Fatal error in PMPI_Init_thread: Other MPI error, error stack:
MPIR_Init_thread(703)........:
MPID_Init(958)...............:
MPIDI_OFI_mpi_init_hook(1334):
MPIDU_bc_table_create(444)...:
```

Unlike SLES11, you cannot run MPI-based executables on the SLES12 login nodes, even if you are just using a single process.  You must run MPI-based executables on a compute node, either interactively or via batch.  The `lisf_7_intel_19_1_0_166` modulefile uses `/discover/nobackup/projects/lis/libs/esmf/7.1.0r_intel-19.1.0.166_impi-20.0.0.166_sles12.3/mod/modO/Linux.intel.64.intelmpi.default` and `/discover/nobackup/projects/lis/libs/esmf/7.1.0r_intel-19.1.0.166_impi-20.0.0.166_sles12.3/lib/libO/Linux.intel.64.intelmpi.default`.  So even if you select "serial" at the parallelization prompt, ESMF will bring in MPI, which leads to the error above.

The two solutions to this issue are:

1) Use the `lisf_7_intel_19_1_0_166 modulefile` and always run on a compute node
2) Create a `lisf_7_intel_19_1_0_166_mpiuni modulefile`.  Edit the definitions for `def_lis_modesmf` and `def_lis_libesmf` by changing `intelmpi` to `mpiuni`.  Then rerun `./configure` and `./compile`.

I suspect that most of you want to run LIS in parallel, so you must be careful when using option 2.  When using option 2, you will load `lisf_7_intel_19_1_0_166_mpiuni` for LDT and LVT, and you will load `lisf_7_intel_19_1_0_166` for LIS.

To compile LISF on SLES 11 with the GNU compilers:

* Load the appropriate module: `module load lisf_7_gnu_4_9_2_mpi`
