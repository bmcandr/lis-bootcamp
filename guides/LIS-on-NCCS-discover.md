# Getting set up on NCCS discover

## Logging onto discover SLES12 node:

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

2. From your local computer, log onto discover:

    `$ ssh discover`

3. After you log onto discover, jump to one of the new SLES12 nodes:

    `$ ssh -Y discover-sles12`

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
