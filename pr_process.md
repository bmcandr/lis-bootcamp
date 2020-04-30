# LIS Pull Requests

## Process for checking PR before merging

1. Review PR description on GitHub to understand changes and testing process
2. Review changed files for conformity to Coding and Documentation Conventions (*see LIS Developers' Guide*)
    * Note any corrections that are needed
3. Make testing directory, `$TESTDIR`, for PR on `discover`
    * Work in `$NOBACKUP` directory
4. Clone feature branch from author's fork to `$TESTDIR`:

        git clone -b <branch> <author repo>
5. Configure and compile relevent LIS components
    * Does it work?
    * Try to break it with different compilers/flags
      * How? --  Ask Jim/Eric
    * *NOTE: some PRs were submitted before SLES12 transition -- will need to run on SLES 11 (otherwise will encounter error of missing library/dependency)*
    * Sometimes folks forget to commit files... (e.g., `Filepath`)
      * Go to repo and look at files touched in PR to see if any expected files are missing (e.g., `ldt.config.adoc`)
      * Identify missing file(s) and request authors commit them
6. Run user-provided testcase
    * When running older testcases check out coincident commit to avoid mismatch between LIS and config file
7. Comment on PR to request authors...
    * Commit any missing files
    * Add necessary documentation
    * Correct any code that violates convention
8. Retest PR after author fixes submitted

----

## First Pull Requests with LIS

### \#485

* Clone Sujay's repo --> checkout `feature/ERA5forc`
* Configure and Compile LDT and LIS -- *SUCCESS*
* Run `./LDT ldt.config_noahmp` -- *SUCCESS*
  * Warnings due to missing dataset: `../input/LS_PARAMETERS/topo_parms/SRTM/SRTM30/gt30w120s60.dem`
* Run `./LIS lis.config` -- *SUCCESS*
  * Output in `$PR_TESTCASES/485/testcase/OUTPUT`
  * Initial comparison
    * `nccmp -dfms` reports differences in files, but values look identical?
    * `nccmp -dfmst 0.001` reports files are identical
* Merge conflict in make/Filepath
* **Actions:**
  * Jim will request documentation and conflict resolution from Sujay
    - [ ] Review Jim's suggested fixes

### \#494

* Clone Sujay's repo --> checkout `feature/OzFluxLVT`
* Configure and Compile LVT -- *SUCCESS*
* Run `./LVT lvt.config` -- *SUCCESS*
  * `nccmp -dsmf` has not finished running, but results appear consistent

- [x] **PR MERGED!**

### \#498

* Clone Sujay's repo --> checkout `feature/LVTmi`
* Configure and Compile LVT -- *SUCCESS*
* Run `./LVT lvt.config` -- *SUCCESS*
  * `[WARN] LIS file ./LIS_OUTPUT/SURFACEMODEL/200202/LIS_HIST_200202010000.d01.nc does not exist` caused by lvt.config specifying end date as 02/01/2002 instead of 01/31/2002
    * Comment on PR to let Sujay know about error in lvt.config
  * Output (`LVT_ MI_FINAL.200202010000.do1.nc`) looks good compared to Sujay's file
    * `nccmp` reports identical output
* **Actions:**
  - [x] Comment on PR to request fixes:
    * Fix: set end date in lvt.config to 01/31/2002 from 02/01/2002
      * Acceptable warning, may not need repair
  - [ ] Review documentation?
    * Add documentation for "Mutual information:" in `lvt/core/LVT_readMetricsAttributes.F90` to `lvt/configs/lvt.config.adoc`
    * Remove debugging print statements in `LVT_mutualinformationMOD` 630-635
    * look at pluginIndices to ensure numbers are in order
    * look at first line of lvt.config
      * remove `ru` before comment marker

### \#503

* Clone Sujay's repo --> checkout `feature/entropyLVT`
* Configure and Compile LVT -- *SUCCESS*
* Run `./LVT lvt.config` -- *PARTIAL SUCCESS*
  * `[WARN] LIS file ./LIS_OUTPUT/SURFACEMODEL/200202/LIS_HIST_200202010000.d01.nc does not exist` caused by lvt.config specifying end date as 02/01/2002 instead of 01/31/2002. Change to lvt.config needed.
  * `LVT_InformationEntropy.F90` should probably be renamed `LVT_InformationEntropyMod.F90`
    * See line 7
  * `nccmp -dmfs LVT_CE_FINAL.200202010000.d01.nc ../../from_svkumar/STATS.ALEXI/LVT_CE_FINAL.200202010000.d01.nc` results in differences:

```bash
DIFFER : VARIABLE : SD_Qle_v_Qle : DOES NOT EXIST IN "../../from_svkumar/STATS.ALEXI/LVT_CE_FINAL.200202010000.d01.nc"

DIFFER : VARIABLE : COUNT_SD_Qle_v_Qle : DOES NOT EXIST IN "../../from_svkumar/STATS.ALEXI/LVT_CE_FINAL.200202010000.d01.nc"
```

  * remove debugging print statements in both 498 and 503
  * `log2()` function in both 498 and 503 -- may cause namespace conflicts
    * Maybe move it to a central module in `lvt/metrics` -- start of a collection of similar functions?
    * Accept PR, create an issue about this
  * Why is `if 0` fenced code in `LVT_InformationEntropy.F90` included?
* **Actions:**
  - [ ] Comment on PR to request fixes:
    * Fix: set end date in lvt.config to 01/31/2002 from 02/01/2002
    * Add documentation for "Information entropy:" and "Conditional entropy:" to `lvt/configs/lvt.config.adoc`

----

## Questions/Issues and Solutions

* What are criteria for success? Compile? Run? Compare outputs?
* What is the best process to compare with 'targets' provided by author of PR?
  * Use `nccmp` to compare files with flags:
    * `-d` : compare data
    * `-m` : compare metadata
    * `-s` : report identical files
    * `-f` : force continue after difference found
* How do you stress test PR? Last meeting: "try to break it with different compilers/flags"
  * Example from PR #191: "Where and how did you run? SLES11 or SLES12, with -O2 or -g?"
  * Run full suite of testcases? On both SLES11/12 and Intel/GNU??
  * Match output then recompile with different flags
    * Prompt about optimization level during config
    * SLES 11 has component for GNU compiler and used different module to load it
      * Jim will share module for GNU compiler with me (GNU ver. 4 -- "tried and true")
        * FYI: LVT does not play well with GNU compiler
          * It *will* compile with optimization level 0...
* When `<LIS-component>/metforcing/readcrd_<module>.F90` files are modified, strings in calls to `<module>_ConfigFindLabel()` must be documented in `<LIS-component>/configs/<LIS-component>config.adoc`
  * See PR #485 for examples
* When would you run `make clean` or `make real clean`?
* How do I get GrADS working? Running into font errors...
  * Jim will send me fix
* Some symlinks point to datasets...when should those be copied into testcase "safe"?
  * See #143, `WRF-4.0.1/` is linked to `$NOBACKUP/projects/lis_aist17/zwang/WRF-4.0.1`
* What to do with PRs labeled "not ready" or "LIS 7.4"?
* If new module, ensure filename matches module name given at declaration (~ line 7)
