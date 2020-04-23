# LIS Pull Requests

### Process:

1. Make directory for PR
2. Clone feature branch from author's fork
  * `git clone -b <branch> <repo>`
3. Test compiling
  * Does it work?
  * Try to break it with different compilers/flags
    * How?
  * *NOTE: some PRs were submitted before SLES12 transition -- will need to run on SLES 11 (otherwise will encounter error of missing library/dependency)*
  * Sometimes folks forget to commit files...
    * Go to repo and look at files touched in PR to see if any are missing
    * Identify missing file(s) and request authors commit it/them

  * Is documentation missing? --> Report back to author and request fix
    * PRs not accepted without documentation!
4. Run user-provided testcase
  * When running older testcases check out coincident commit to avoid mismatch between LIS and config file

----

#### \#485
* Clone Sujay's repo --> checkout `feature/ERA5forc`
* Configure and Compile LDT and LIS -- *SUCCESS*
  * *If PR only touches one LIS component, only compile that?*
* Run `./LDT ldt.config_noahmp` -- *SUCCESS*
  * Run full suite of testcases? On both SLES11/12 and Intel/GNU??
* Run `./LIS lis.config` -- *SUCCESS*
  * Output in `$PR_TESTCASES/485/testcase/OUTPUT`
  * Initial comparison
    * `nccmp -dfms` reports differences in files, but values look identical?
    * `nccmp -dfmst 0.001` reports files are identical

#### \#494
* Clone Sujay's repo --> checkout `feature/OzFluxLVT`
* Configure and Compile LVT -- *SUCCESS*
  * *If PR only touches one LIS component, only compile that?*
* Run `./LVT lvt.config` -- *SUCCESS*
  * `nccmp -dsmf` has not finished running, but results appear consistent

#### \#498
* Clone Sujay's repo --> checkout `feature/LVTmi`
* Configure and Compile LVT -- *SUCCESS*
* Run `./LVT lvt.config` -- *SUCCESS*
  * `[WARN] LIS file ./LIS_OUTPUT/SURFACEMODEL/200202/LIS_HIST_200202010000.d01.nc does not exist` caused by lvt.config specifying end date as 02/01/2002 instead of 01/31/2002 -- normal warning?
  * Output (`LVT_ MI_FINAL.200202010000.do1.nc`) looks good compared to Sujay's file
    * `nccmp` reports identical output


#### \#503
* Clone Sujay's repo --> checkout `feature/ERA5forc`
* Configure and Compile LVT -- *SUCCESS*
  * *If PR only touches one LIS component, only compile that?*
* Run `./LVT lvt.config`
  *

----

### Questions/Issues

* What are criteria for success? Compile? Run? Compare outputs?
* What is the best process to compare with 'targets' provided by author of PR?
* How do you stress test PR? Last meeting: "try to break it with different compilers/flags"
* Run full suite of testcases? On both SLES11/12 and Intel/GNU??
