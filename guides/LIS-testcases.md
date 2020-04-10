# Running LIS Test Cases on NCCS discover

***NOTE: RUNNING THE THIS TUTORIAL WITH THE LATEST MASTER BRANCH WILL FAIL!***

The test cases are associated with an earlier commit and will fail if attempted with the master branch.  Go to the [LIS Test Cases page](https://lis.gsfc.nasa.gov/tests/lis) and look at the note at the bottom for the appropriate commit ID to checkout (440a319 as of 4/10/2020).

Example:
1. Change directory into LISF repo with `cd $WORKING` where `$WORKING` is LISF/
2. Check out appropriate commit with `git checkout <commit id>`

----

### Download and Extract Test Case Data

The files needed to run the LIS/LDT/LVT test cases are available for download on the [LIS Test Cases page](https://lis.gsfc.nasa.gov/tests/lis). The tarballs contain both the data and config files needed to run the test cases.

Use the `curl` utility to download the .tgz files:
1. Go to the [LIS Test Cases page](https://lis.gsfc.nasa.gov/tests/lis) and copy the URL to the test case you want to run.
2. In the terminal, change directories to the directory where you want to store the test case data (i.e., create a `$HOME/test-cases` and `cd` into that directory).
3. Download the file using `curl`:

  `curl <url> .`

Repeat steps 1-3 to download each test case's tarball.

Extract each .tgz file with the following command, replacing `<filename>` with the appropriate filename:

* `gzip -dc <filename> | tar xf -`

----

# Notes/Questions from Running Test Cases

* LDT Test Case 1: instructions describe initial Soil texture spatial transform as `tile`, but it is actually `mode` -- change file or instructions?


## Notes from Jim re: Troubleshooting

You will run into one problem.  The older SLES11 nodes allow you to run a single-process MPI run right on the login node like this: `./LIS`; SLES12 does not.  One option is to run interactively on a compute node.  The other is create a new modulefile and recompile LDT, LIS, and LVT without MPI support.

Create a new modulefile that is a copy of lisf_7_intel_19_1_0_166.  Let's call it lisf_7_intel_19_1_0_166_mpiuni.  Edit that file, changing "intelmpi" to "mpiuni" in definitions for both def_lis_modesmf and def_lis_libesmf.

Then `module purge && module load lisf_7_intel_19_1_0_166_mpiuni`

Finally rerun ./configure and ./compile for LDT, LIS, and LVT.  Select "serial" at the Parallelization prompt.

You will be able to play along with the instructions in the overview slides right on the SLES12 login node.

********
UPDATE: configure failed because $LIS_JASPER was undefined in module; added line `setenv LIS_JASPER $def_lis_jasper` and configure worked.

Compile failed: ld: cannot find -ljasper

Compile failed:
Makefile:139: recipe for target 'write_GCOMW_AMSR2L3SNDobs.o' failed
make: *** [write_GCOMW_AMSR2L3SNDobs.o] Error 139

[ERR] Compile failed

********

Solution:

The actual source of the problem is that commit 440a319 and the updates for SLES12 are not compatible.  Sorry for not realizing that earlier.  Since this is just a test, you can either use the SLES11 nodes and environment or hack the three Config.pl script used by ./configure.  I suggest the SLES11/Intel route because these tests can run directly on the login node.  The SLES11/Intel18 modulefile is also on GitHub in the env directory.  Just put the lisf_7_intel_18_0_3_222 in your $HOME/privatemodules directory.  Then log back onto discover.  `cat /etc/SuSE-release` should confirm that you are on SLES11.  Then load the lisf_7_intel_18_0_3_222 environment and rerun the ./configure and ./compile scripts.
