# Intro to LIS on NCCS discover

This document provides information to get new users/contributors to LIS up and running on NASA's [Discover](https://www.nccs.nasa.gov/systems/discover) high-performance computing environment.

## Table of Contents

* [Accessing Discover](#accessing-discover)
* [Setting up your Environment](#setting-up-your-environment-on-discover)
* [LIS Test Cases](#lis-test-cases)

-----

## Accessing Discover

### Prerequisites

* NASA IdMax identity
* RSA Key
    * Submit [NAMS](https://idmax.nasa.gov/) request *Agency RSA SecurID Token* and follow included instructions
* An account with [NASA Center for Climate Simulation](https://www.nccs.nasa.gov/nccs-users/instructional/account-set-up) (NCCS)
* Discover permissions
    * Ask your PI
* Familiarity with using the terminal or shell (e.g., bash)
    * Need a refresher? [Try this!](http://swcarpentry.github.io/shell-novice/)

### Connecting to Discover

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
    ssh discover
    ```

    <!-- SLES11 will be deprecated eventually and this will need to be updated -->

    * To then jump to one of the new SLES12 nodes enter:

        ```sh
        ssh -Y discover-sles12
        ```

    \**Windows users will have to install a terminal emulator such as [PuTTY](https://www.putty.org/) or [MobaXterm](https://mobaxterm.mobatek.net/download.html)*.

3. At the `PASSCODE` prompt, enter the randomly generated 8-digit RSA key that is displayed after entering your PIN in the RSA app.

4. At the `Host:` prompt, enter `discover`.

5. At the `Password` prompt, enter your NCCS LDAP password.

If the connection was successful you will see something like this:

```sh
Your password will expire in X day(s).
Last login: 1 Jan 1 12:00:00 1970 from login1.nccs.nasa.gov

*******************************************************************************

Please see the NCCS website for current status:

http://www.nccs.nasa.gov/sys.status/motd.html

*******************************************************************************

<userid>@discoverNN:~>
```

NCCS also provides [these instructions](https://www.nccs.nasa.gov/nccs-users/instructional/logging-in/bastion-host) for connecting to their systems.

-----

## Setting up your environment on Discover

1. Change directories to your `$HOME` directory:

    ```sh
    cd $HOME
    ```

2. We suggest that you start with a clean login environment. To do so, move your `.profile `and `.bashrc` files to another directory for safekeeping (assuming that you use bash). If your `.bashrc` file does not run any `module load` commands nor sets any critical environment variables like `$PATH` or `$LD_LIBRARY_PATH`, then you should be safe leaving your `.bashrc` file alone.

    ```sh
    mkdir profile_temp
    mv .profile .bashrc profile_temp/
    ```

3. Create a new `.profile` in your `$HOME` directory and add the following to it:

    <!-- replace with David Mocko's autoloader for SLES11/12? -->

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

4. Make a directory called `privatemodules` in your `$HOME` directory:

    ```sh
    mkdir privatemodules
    ```

5. Copy the modulefile for SLES11 and/or SLES12 into `privatemodules`:

    <!-- place modulefiles in shared location on discover? -->

    * SLES11*:

    ```sh
    cp /path/to/lisf_7_intel_18 privatemodules/
    ```

    * SLES12:

    ```sh
    cp /path/to/lisf_7_intel_19_1_0_166 privatemodules/
    ```

    \**SLES11 will soon be deprecated and SLES12 will be the only option.*

6. Load the modulefile.  You will have to do this step every time you log onto discover-sles12

    <!-- replace with SLES11 path? -->

    ```sh
    module load lisf_7_intel_19_1_0_166
    ```

7. You are now ready to clone [LISF](https://github.com/NASA-LIS/LISF.git) and practice compiling. We suggest that you work in `/discover/nobackup/<userid>`.  The quota for your `$HOME` directory is pretty small.

    ```sh
    cd $NOBACKUP
    ```

Follow the [Working with GitHub](https://github.com/NASA-LIS/LISF/blob/master/docs/working_with_github/working_with_github.adoc) documentation to clone the LIS sourcecode into the directory you created.

*New to Git and GitHub? Need a refresher? Try these links:*

* *[Intro to Git](https://git-scm.com/book/en/v2)*
* *[GitHub Guide (Text)](https://help.github.com/en/github)*
* *[GitHub Guide (Videos)](https://www.youtube.com/playlist?list=PLg7s6cbtAD15G8lNyoaYDuKZSKyJrgwB-)*

-----

### LIS Test Cases

You are now ready to work through the LIS test cases. This process will verify that your working environment is set up properly and walk through the process to configure, compile, and run LIS. Instructions and files needed to run the test cases can be found [here](https://lis.gsfc.nasa.gov/tests/lis).

*Note: you must check out the specific commit indicated at the bottom of that page or the testcases may not run successfully.*

-----

### Issues Running LIS on SLES12

***ALERT: This section is incomplete***

Notes from Jim:

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