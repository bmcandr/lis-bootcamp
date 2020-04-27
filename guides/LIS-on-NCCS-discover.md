# Intro to LIS on NCCS discover

## Basic Setup

1. First on your local computer, add this to your `$HOME/.ssh/config` file:
```
  Host discover
  #LogLevel Quiet
  User bmcandr1
  ProxyCommand ssh login.nccs.nasa.gov direct %h
  ForwardX11 yes
  ForwardX11Trusted yes
  Protocol 2
```

  This configuration file makes it easier to connect to discover.

2. From your local computer, ssh onto discover: `$ ssh discover`

    * To jump to one of the new SLES12 nodes: `$ ssh -Y discover-sles12`

## Setting up your environment on discover

1. I suggest that you start with a clean login environment.  So, in your `$HOME` directory, move your .profile and .bashrc files out of the way (I am assuming that you use bash). If your .bashrc file does not run any `module load` commands nor sets any critical environment variables like `$PATH` or `$LD_LIBRARY_PATH`, then you should be safe leaving your .bashrc file alone.

2. Replace contents of .profile with clean version:

  ```
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

3. Make a privatemodules directory

  `$ mkdir privatemodules`

4. Copy our modulefile that for SLES12 *(POINT TO FILE ON REPO?)*

  `$ cp /discover/nobackup/jvgeiger/for/brendan/lisf_7_intel_19_1_0_166 privatemodules/`

5. Load the modulefile.  You will have to do this step every time you log onto discover-sles12

  `$ module load lisf_7_intel_19_1_0_166`

You are now ready to clone LISF and practice compiling.  I suggest that you work in `/discover/nobackup/<userid>`.  The quota for your `$HOME` directory is pretty small.


### Issues with SLES12

We have been having issues running on SLES12; in particular:

```
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
